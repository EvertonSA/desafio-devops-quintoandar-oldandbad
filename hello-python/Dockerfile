FROM ubuntu:16.04
RUN apt update -y && apt install -y python-pip python-dev
WORKDIR /opt/hello-python
COPY ./requirements.txt /opt/hello-python/requirements.txt
RUN pip install -r requirements.txt
COPY . .
ENTRYPOINT ["python"]
CMD ["app.py"]
