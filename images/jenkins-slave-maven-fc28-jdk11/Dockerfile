FROM thmarth/jenkins-slave-base-fc28-jdk11

ENV MAVEN_VERSION=3.5 \
    GRADLE_VERSION=4.2.1 \
    PATH=$PATH:/opt/gradle/bin

# Install Maven
RUN INSTALL_PKGS="maven" && \
    yum install -y $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all -y && \
    mkdir -p $HOME/.m2

# Install gradle
RUN curl -LOk https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip && \
    unzip gradle-${GRADLE_VERSION}-bin.zip -d /opt && \
    rm -f gradle-${GRADLE_VERSION}-bin.zip && \
    ln -s /opt/gradle-${GRADLE_VERSION} /opt/gradle && \
    mkdir -p $HOME/.gradle

COPY contrib/bin/configure-agent /usr/local/bin/configure-agent
COPY contrib/settings.xml $HOME/.m2/
COPY contrib/init.gradle $HOME/.gradle/

RUN chown -R 1001:0 $HOME && \
    chmod -R g+rw $HOME

USER 1001