FROM debian:13.1-slim

ARG GOLANG_VERSION=1.25.2
ARG GOLANG_ARCH=linux-arm64

RUN apt update -y
RUN apt upgrade -y

RUN apt install -y \
    git \
    curl \
    wget \
    ca-certificates \
    gpg \
    gnupg \
    unzip \
    build-essential \
    fish \
    neovim \
    bat \
    htop \
    zoxide \
    php  \
    fzf \
    php-cli  \
    php-mbstring \
    php-xml \
    php-curl

# Install Docker CLI
RUN curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
RUN chmod a+r /etc/apt/keyrings/docker.asc
RUN echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      tee /etc/apt/sources.list.d/docker.list > /dev/null
RUN apt update -y
RUN apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Install Golang
RUN wget https://go.dev/dl/go${GOLANG_VERSION}.${GOLANG_ARCH}.tar.gz
RUN tar -C /usr/local -xzf go${GOLANG_VERSION}.${GOLANG_ARCH}.tar.gz
RUN rm go${GOLANG_VERSION}.${GOLANG_ARCH}.tar.gz

ENV PATH="/usr/local/go/bin:${PATH}"
ENV GOPATH="/root/go"
ENV PATH="${GOPATH}/bin:${PATH}"

# Install PHP and Composer
RUN apt install -y
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install eza
RUN mkdir -p /etc/apt/keyrings
RUN wget -qO- https://raw.githubusercontent.com/eza-community/eza/main/deb.asc | gpg --dearmor -o /etc/apt/keyrings/gierens.gpg
RUN echo "deb [signed-by=/etc/apt/keyrings/gierens.gpg] http://deb.gierens.de stable main" | tee /etc/apt/sources.list.d/gierens.list
RUN chmod 644 /etc/apt/keyrings/gierens.gpg /etc/apt/sources.list.d/gierens.list
RUN apt update -y
RUN apt install -y eza

# Install Oh My Fish
RUN curl https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install > install
RUN fish install --noninteractive --path=~/.local/share/omf --config=~/.config/omf
RUN rm -rf ./install

# Configure zoxide for Fish
RUN mkdir -p /root/.config/fish
RUN echo 'zoxide init fish | source' >> /root/.config/fish/config.fish


# Set up aliases for bat, eza, and zoxide
RUN echo '' >> /root/.config/fish/config.fish
RUN echo '# Aliases' >> /root/.config/fish/config.fish
RUN echo 'alias cat="batcat"' >> /root/.config/fish/config.fish
RUN echo 'alias ls="eza --icons"' >> /root/.config/fish/config.fish
RUN echo 'alias ll="eza -l --icons --git"' >> /root/.config/fish/config.fish
RUN echo 'alias la="eza -la --icons --git"' >> /root/.config/fish/config.fish
RUN echo 'alias lt="eza --tree --icons"' >> /root/.config/fish/config.fish
RUN echo 'alias cd="z"' >> /root/.config/fish/config.fish
RUN echo 'alias cdi="zi"' >> /root/.config/fish/config.fish
RUN echo 'alias vi="nvim"' >> /root/.config/fish/config.fish
RUN echo 'alias vim="nvim"' >> /root/.config/fish/config.fish

# Set Fish as default shell
ENV SHELL=/usr/bin/fish
SHELL ["/usr/bin/fish", "-c"]

CMD ["/usr/bin/fish"]