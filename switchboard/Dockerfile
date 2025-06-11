FROM ghcr.io/hassio-addons/base-python:16.1.4

WORKDIR /app
COPY switchboard.py .

RUN pip install requests

COPY run.sh .
RUN chmod a+x ./run.sh

CMD [ "./run.sh" ]