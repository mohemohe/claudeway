FROM ubuntu:24.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV ASDF_DIR=/opt/asdf
ENV ASDF_DATA_DIR=/opt/asdf
ENV PATH="${ASDF_DIR}/bin:${ASDF_DIR}/shims:${PATH}"
ENV TMPDIR=/tmp
ENV HOME=/root

# Install basic dependencies
RUN apt-get update && apt-get install -y \
    curl \
    git \
    build-essential \
    libssl-dev \
    zlib1g-dev \
    libbz2-dev \
    libreadline-dev \
    libsqlite3-dev \
    wget \
    llvm \
    libncurses5-dev \
    libncursesw5-dev \
    xz-utils \
    tk-dev \
    libffi-dev \
    liblzma-dev \
    unzip \
    rsync \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# Install asdf
RUN git clone https://github.com/asdf-vm/asdf.git ${ASDF_DIR} --branch v0.14.0 && \
    echo '. /opt/asdf/asdf.sh' >> /etc/bash.bashrc && \
    echo '. /opt/asdf/completions/asdf.bash' >> /etc/bash.bashrc && \
    echo '. /opt/asdf/asdf.sh' >> /etc/profile && \
    echo '. /opt/asdf/completions/asdf.bash' >> /etc/profile

# Install commonly used asdf plugins
RUN . ${ASDF_DIR}/asdf.sh && \
    asdf plugin add nodejs && \
    asdf plugin add python && \
    asdf plugin add golang && \
    asdf plugin add ruby && \
    asdf plugin add java

# Create host directory for copy operations
RUN mkdir -p /host

# Copy entrypoint script
COPY entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod +x /usr/local/bin/entrypoint.sh

# Set working directory
WORKDIR /workspace

# Set entrypoint
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

# Default command
CMD ["/bin/bash", "-l"]