ARG PYTHON_VERSION=3.11

FROM --platform=linux/amd64 public.ecr.aws/lambda/python:${PYTHON_VERSION}

WORKDIR /tmp

RUN yum -y install git && \
    pip install -U pip && \
    pip install "git+https://github.com/gadomski/titiler.git@9f6dc809259c07a7fc8acf8beb820d8e622c6985#egg=titiler.core&subdirectory=src/titiler/core" \
    "git+https://github.com/gadomski/titiler.git@9f6dc809259c07a7fc8acf8beb820d8e622c6985#egg=titiler.application&subdirectory=src/titiler/application" \
    "mangum>=0.10.0" -t /asset --no-binary pydantic && \
    yum remove -y git

# Reduce package size and remove useless files
RUN cd /asset && find . -type f -name '*.pyc' | while read f; do n=$(echo $f | sed 's/__pycache__\///' | sed 's/.cpython-[0-9]*//'); cp $f $n; done;
RUN cd /asset && find . -type d -a -name '__pycache__' -print0 | xargs -0 rm -rf
RUN cd /asset && find . -type f -a -name '*.py' -print0 | xargs -0 rm -f
RUN find /asset -type d -a -name 'tests' -print0 | xargs -0 rm -rf
RUN rm -rdf /asset/numpy/doc/ /asset/boto3* /asset/botocore* /asset/bin /asset/geos_license /asset/Misc

COPY lambda/handler.py /asset/handler.py

CMD ["echo", "hello world"]