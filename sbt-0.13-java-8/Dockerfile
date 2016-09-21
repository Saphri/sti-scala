FROM openshift/base-centos7
# TODO: Take a look at https://github.com/heroku/sbt-pkg-builder and https://github.com/heroku/heroku-buildpack-scala for pre-caching

MAINTAINER Martin Nyholm Jelle <mnj@miracle.dk>

ENV SBT_VERSION 0.13.9
ENV JAVA_VERSION 1.7.0

LABEL io.k8s.description="Build and run Scala applications" \
      io.k8s.display-name="SBT 0.13.9" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,scala,sbt" \
      io.openshift.min-memory="512Mi"

ADD bintray-sbt-rpm.repo /etc/yum.repos.d/bintray-sbt-rpm.repo

RUN yum install -y --enablerepo=centosplus \
    java-${JAVA_VERSION}-openjdk java-${JAVA_VERSION}-openjdk-devel sbt-${SBT_VERSION} thrift && \
    yum clean all -y

RUN sbt -ivy /opt/app-root/src/.ivy2 about
RUN chown -R 1001:0 /opt/app-root/src/.ivy2; chmod -R g+rw /opt/app-root/src/.ivy2

COPY ./s2i/bin/ $STI_SCRIPTS_PATH

EXPOSE 8080

USER 1001

CMD $STI_SCRIPTS_PATH/usage
