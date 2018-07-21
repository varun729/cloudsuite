FROM cloudsuite/java:openjdk8-aarch64
LABEL maintainer="Mark Sutherland <mark.sutherland@epfl.ch>"

RUN apt-get update && apt-get install -y \
	ant \
	build-essential \
        curl \
	wget

# Setup Faban 

RUN wget http://faban.org/downloads/faban-kit-latest.tar.gz
RUN tar zxvf faban-kit-latest.tar.gz
RUN sed -e 's/\/bin\/sh/\/bin\/bash/g' -i /faban/master/bin/startup.sh
RUN sed -e 's/\/bin\/sh/\/bin\/bash/g' -i /faban/master/bin/catalina.sh

COPY files/web20_benchmark /web20_benchmark

WORKDIR /web20_benchmark


# Build the Faban benchmark and the user-generation tool
RUN ant deploy.jar
RUN ant usergen-jar

# Copy files to their required locations
RUN cp /web20_benchmark/build/Web20Driver.jar /faban/benchmarks/

COPY files/usersetup.properties /faban/usersetup.properties

ADD bootstrap.sh /etc/bootstrap.sh
RUN chown root:root /etc/bootstrap.sh
RUN chmod 700 /etc/bootstrap.sh

ENTRYPOINT ["/etc/bootstrap.sh"]
