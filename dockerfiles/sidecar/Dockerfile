FROM debian:13.1-slim

ARG GOLANG_VERSION=1.25.2

RUN apt update -y
RUN apt upgrade -y

RUN apt install -y \
    git \
    curl \
    wget \
    ca-certificates \
    gnupg \
    unzip \
    build-essential \
    fish \
    neovim \
    bat \
    htop

# Install Golang
RUN wget https://go.dev/dl/go${GOLANG_VERSION}.linux-arm64.tar.gz \
    && tar -C /usr/local -xzf go${GOLANG_VERSION}.linux-arm64.tar.gz \
    && rm go${GOLANG_VERSION}.linux-arm64.tar.gz

ENV PATH="/usr/local/go/bin:${PATH}"
ENV GOPATH="/root/go"
ENV PATH="${GOPATH}/bin:${PATH}"

# Install PHP and Composer
RUN apt install -y php php-cli php-mbstring php-xml php-curl \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install Rust and cargo (needed for eza)
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"

# Install zoxide
RUN curl -sS https://raw.githubusercontent.com/ajeetdsouza/zoxide/main/install.sh | bash

# Install eza
RUN cargo install eza

# Install Oh My Fish
RUN fish -c "curl https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install | fish" || true

# Configure zoxide for Fish
RUN mkdir -p /root/.config/fish \
    && echo 'zoxide init fish | source' >> /root/.config/fish/config.fish

# Set up aliases for bat, eza, and zoxide
RUN echo '' >> /root/.config/fish/config.fish \
    && echo '# Aliases' >> /root/.config/fish/config.fish \
    && echo 'alias cat="bat"' >> /root/.config/fish/config.fish \
    && echo 'alias ls="eza --icons"' >> /root/.config/fish/config.fish \
    && echo 'alias ll="eza -l --icons --git"' >> /root/.config/fish/config.fish \
    && echo 'alias la="eza -la --icons --git"' >> /root/.config/fish/config.fish \
    && echo 'alias lt="eza --tree --icons"' >> /root/.config/fish/config.fish \
    && echo 'alias cd="z"' >> /root/.config/fish/config.fish \
    && echo 'alias cdi="zi"' >> /root/.config/fish/config.fish \
    && echo 'alias vi="nvim"' >> /root/.config/fish/config.fish \
    && echo 'alias vim="nvim"' >> /root/.config/fish/config.fish

# Set Fish as default shell
ENV SHELL=/usr/bin/fish
SHELL ["/usr/bin/fish", "-c"]

CMD ["/usr/bin/fish"]