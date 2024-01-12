####################################################################################
#### Use base image to compile AlsavoCtrl
####################################################################################
FROM debian AS build

#### Install build dependencies for AlsavoCtrl
RUN apt-get update && \
        apt-get install -y cmake && \
        apt-get install -y make && \
        apt-get install -y g++ && \
        apt-get install -y git
		
#### Copy AlsavoCtrl files to image
RUN git clone https://github.com/philipsoeberg/alsavo.git

WORKDIR /alsavo

#### Build AlsavoCtrl
RUN mkdir build

WORKDIR /alsavo/build

RUN cmake ../AlsavoCtrl
RUN make

####################################################################################
#### Build final image
####################################################################################
FROM debian as base

RUN apt-get update && apt install -y python3-paho-mqtt

#### Define variables
ENV mqtt_server=None
ENV mqtt_server_port=1883
ENV mqtt_server_user=None
ENV mqtt_server_password=None
ENV alsavo_password=None
ENV alsavo_serial=None
ENV alsavo_port=None
ENV alsavo_address=None
ENV publish_interval=300
ENV log_size=1000

#### Copy files
RUN mkdir app data

COPY --from=build \
    ./alsavo/build/AlsavoCtrl \
    ./app/
COPY /app.py /app/app.py
COPY /config.py /app/config.py

#### Set app.py as image main process
CMD [ "python3", "/app/app.py"]
