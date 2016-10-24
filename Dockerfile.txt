## Pavel-Kaminsky.com

FROM tensorflow/tensorflow

RUN apt-get -qq update

# Java 8
RUN \
        sudo apt-get install -y software-properties-common && \
        sudo add-apt-repository ppa:webupd8team/java && \
        sudo apt-get update && \
        sudo echo oracle-java7-installer shared/accepted-oracle-license-v1-1 select true | \
        sudo /usr/bin/debconf-set-selections

ENV JAVA_HOME /usr/lib/jvm/java-8-oracle

# Bazel
RUN \
		echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list && \
        curl https://bazel.io/bazel-release.pub.gpg | sudo apt-key add - && \
        sudo apt-get update && \
        sudo apt-get install -y bazel && \
        sudo apt-get upgrade -y bazel

# Git
RUN \
        sudo apt-get install -y git && \
        git clone https://github.com/tensorflow/models.git ./notebooks/tesnorflow-models

EXPOSE 8888 6006
WORKDIR /
CMD sh run_jupyter.sh &  python ./usr/local/lib/python2.7/dist-packages/tensorflow/tensorboard/tensorboard.py --logdir=tmp