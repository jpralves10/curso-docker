# Build > docker image build -t ex-build-dev .
# Run   > docker container run -it -v %cd%:/app -p 80:8000 --name python-server ex-build-dev
# Logs  > docker container run -it --volumes-from=python-server debian cat /log/http-server.log

FROM python:3.6
LABEL maintainer "Aluno Cod3r <aluno at cod3r.com.br>"

RUN useradd www && \
    mkdir /app && \
    mkdir /log && \
    chown www /log

USER www
VOLUME /log
WORKDIR /app
EXPOSE 8000

ENTRYPOINT [ "/usr/local/bin/python" ]
CMD [ "run.py" ]