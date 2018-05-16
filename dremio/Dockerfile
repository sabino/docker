FROM openjdk:8
MAINTAINER Felipe Sabino <felipe@sabino.pro>

RUN apk update && \
	apk add gcc g++ make unzip git wget ca-certificates --no-cache && \
	rm -fr /var/cache/apk/*

ENV MAVEN_VERSION 3.3.9
ENV MAVEN_HOME /usr/lib/mvn
ENV PATH $MAVEN_HOME/bin:$PATH
ENV MAVEN_OPTS -Xmx768m -XX:+UseConcMarkSweepGC -XX:MaxPermSize=128m -XX:+CMSPermGenSweepingEnabled

RUN wget http://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz && \
	tar -zxvf apache-maven-$MAVEN_VERSION-bin.tar.gz && \
	rm apache-maven-$MAVEN_VERSION-bin.tar.gz && \
	mv apache-maven-$MAVEN_VERSION /usr/lib/mvn

RUN cd /tmp && \
    wget http://github.com/google/protobuf/releases/download/v2.5.0/protobuf-2.5.0.zip && \
    unzip protobuf-2.5.0.zip && \
    cd protobuf-2.5.0 && \
    ./configure --prefix=/usr --disable-shared && \
    make && make install && \
    cd /tmp/ && rm -rf *

RUN git clone https://github.com/dremio/dremio-oss.git /dremio-git && \
	cd /dremio-git && \
	mvn clean install -DskipTests && \
	mv /dremio-git/distribution/server/target/dremio-community*/dremio-community*/ /dremio && \
	rm -rf /dremio-git

WORKDIR /dremio

EXPOSE 9047 31010

CMD /dremio/bin/dremio start && tail -f /dremio/log/server.out
