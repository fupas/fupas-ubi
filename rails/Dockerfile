FROM registry.access.redhat.com/ubi8/s2i-base

# This image provides a Ruby environment you can use to run your Ruby
# applications.

EXPOSE 8080

ENV RUBY_MAJOR_VERSION=2 \
    RUBY_MINOR_VERSION=6 \
    BUNDLER_VERSION='2.1.4' \
    YARN_VERSION='1.22.4' \
    BUNDLE_DIR='./bundle' \
    PUMA_OPTIONS='--config ../etc/puma.cfg'

ENV NODEJS_VERSION=10 \
    NPM_RUN=start \
    NAME=nodejs \
    NPM_CONFIG_PREFIX=$HOME/.npm-global \
    PATH=$HOME/node_modules/.bin/:$HOME/.npm-global/bin/:$PATH

ENV RUBY_VERSION="${RUBY_MAJOR_VERSION}.${RUBY_MINOR_VERSION}" \
    RUBY_SCL_NAME_VERSION="${RUBY_MAJOR_VERSION}${RUBY_MINOR_VERSION}"

ENV RUBY_SCL="ruby-${RUBY_SCL_NAME_VERSION}" \
    IMAGE_NAME="fupas/rails-${RUBY_VERSION}" \
    SUMMARY="Platform for building and running RAILS applications" \
    DESCRIPTION="Ruby $RUBY_VERSION available as container is a base platform for \
building and running various RAIL applications and frameworks. \
Ruby is the interpreted scripting language for quick and easy object-oriented programming. \
It has many features to process text files and to do system management tasks (as in Perl). \
It is simple, straight-forward, and extensible."

LABEL summary="$SUMMARY" \
      description="$DESCRIPTION" \
      io.k8s.description="$DESCRIPTION" \
      io.k8s.display-name="Ruby ${RUBY_VERSION}" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,ruby,ruby${RUBY_SCL_NAME_VERSION},${RUBY_SCL}" \
      com.redhat.component="${RUBY_SCL}-container" \
      name="${IMAGE_NAME}" \
      version="1" \
      com.redhat.license_terms="https://www.redhat.com/en/about/red-hat-end-user-license-agreements#UBI" \
      usage="s2i build https://github.com/sclorg/s2i-ruby-container.git \
--context-dir=${RUBY_VERSION}/test/puma-test-app/ ${IMAGE_NAME} ruby-sample-app" \
      maintainer="SoftwareCollections.org <sclorg@redhat.com>"

# Install ruby
RUN yum -y module enable ruby:$RUBY_VERSION && \
    INSTALL_PKGS=" \
    ruby \
    ruby-devel \
    rubygem-rake \
    rubygem-bundler \
    redhat-rpm-config \
    " && \
    yum install -y --setopt=tsflags=nodocs ${INSTALL_PKGS} && \
    yum -y clean all --enablerepo='*' && \
    rpm -V ${INSTALL_PKGS}

# Install node.js
RUN yum -y module enable nodejs:$NODEJS_VERSION && \
    INSTALL_PKGS="nodejs npm nodejs-nodemon nss_wrapper" && \
    ln -s /usr/lib/node_modules/nodemon/bin/nodemon.js /usr/bin/nodemon && \
    yum remove -y $INSTALL_PKGS && \
    yum install -y --setopt=tsflags=nodocs $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum -y clean all --enablerepo='*'
    
# Install yarn
RUN npm install yarn@$YARN_VERSION -g

# Update the image
RUN yum -y update

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Copy extra files to the image.
COPY ./root/ /

# Drop the root user and make the content of /opt/app-root owned by user 1001
RUN chown -R 1001:0 ${APP_ROOT} && chmod -R ug+rwx ${APP_ROOT} && \
    rpm-file-permissions

USER 1001

# Install bundler
RUN gem install bundler -v $BUNDLER_VERSION --no-document

# Set the default CMD to print the usage of the language image
CMD $STI_SCRIPTS_PATH/usage
