# Use uma imagem base do Ubuntu
FROM ubuntu:latest

# Atualize o sistema e instale curl
RUN apt-get update && apt-get install -y curl

# Crie um usuário não-root e um diretório para o VS Code Server
RUN useradd -m vscode
RUN mkdir /home/vscode/workplace
RUN chown -R vscode:vscode /home/vscode/workplace

# Baixe e instale o VS Code Server
RUN curl -fsSL https://code-server.dev/install.sh | sh

# Instale o NVM
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash

# Instale o Caddy
RUN apt-get install -y debian-keyring debian-archive-keyring apt-transport-https
RUN curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
RUN echo "deb [signed-by=/usr/share/keyrings/caddy-stable-archive-keyring.gpg] https://dl.cloudsmith.io/public/caddy/stable/deb/debian any-version main" | tee /etc/apt/sources.list.d/caddy-stable.list
RUN apt-get update
RUN apt-get install caddy

# Instale o Chromium
RUN apt-get install -y chromium-browser

# Adicione o arquivo de configuração do Caddy
RUN echo "code.barbudas.com {\n  reverse_proxy 127.0.0.1:8080\n}" > /etc/caddy/Caddyfile

# Mude para o usuário não-root
USER vscode

# Defina a variável de ambiente CODE_SERVER_USER_DATA_DIR
ENV CODE_SERVER_USER_DATA_DIR=/home/vscode/workplace

# Exponha a porta 8080
EXPOSE 8080

# Inicie o VS Code Server e o Caddy quando o container for iniciado
CMD /bin/bash -c "code-server --bind-addr 0.0.0.0:8080 & caddy run"