FROM alpine:edge
MAINTAINER Guillaume Scheibel <guillaume.scheibel@gmail.com>

RUN echo 'http://dl-4.alpinelinux.org/alpine/edge/main' > /etc/apk/repositories
RUN apk add --update bash gcc g++ make openjdk7 ruby ruby-dev ruby-nokogiri asciidoctor curl python python-dev unzip findutils wget tar ttf-dejavu \
    && rm -rf /var/cache/apk/*

ENV JAVA_HOME /usr/lib/jvm/java-1.7-openjdk/
ENV PATH $PATH:/fopub/bin
ENV BACKENDS /asciidoctor-backends

RUN gem install --no-ri --no-rdoc asciidoctor-diagram && \
    gem install --no-ri --no-rdoc asciidoctor-epub3 --version 1.0.0.alpha.2 && \
    gem install --no-ri --no-rdoc asciidoctor-pdf --version 1.5.0.alpha.7 && \
    gem install --no-ri --no-rdoc asciidoctor-confluence && \
    gem install --no-ri --no-rdoc coderay pygments.rb thread_safe epubcheck kindlegen && \
    gem install --no-ri --no-rdoc slim && \
    gem install --no-ri --no-rdoc haml tilt && \
    mkdir $BACKENDS && \
    mkdir /documents && \
    (curl -LkSs https://api.github.com/repos/asciidoctor/asciidoctor-backends/tarball | tar xfz - -C $BACKENDS --strip-components=1)

# Install blockdiag, seqdiag, actdiag and nwdiag diagram tools
RUN wget https://bitbucket.org/pypa/setuptools/raw/bootstrap/ez_setup.py -O - | python
RUN easy_install "blockdiag[pdf]"
RUN easy_install seqdiag
RUN easy_install actdiag
RUN easy_install nwdiag

RUN (curl -s get.gvmtool.net | bash)
RUN ["/bin/bash", "-c", "source /root/.gvm/bin/gvm-init.sh"] 
RUN echo "gvm_auto_answer=true" >> /root/.gvm/etc/config
RUN ["/bin/bash", "-c", "-l", "gvm install lazybones"]

WORKDIR /documents
VOLUME /documents

CMD ["/bin/bash"]
