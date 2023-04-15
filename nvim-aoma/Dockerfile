# Docker file for Neovim Aoma development.
#
# @author Maciej Bedra

# Base Neovim image.
FROM mashmb/nvim:dev

# Neovim COC extensions for development: Go, Python3.
ARG COC='coc-pyright coc-python coc-go'

# Install SDK.
RUN apt-get update && apt-get -y install watchman golang python3-venv && go install golang.org/x/tools/gopls@latest && npm install -g typescript

# Create directory for virtual environments.
RUN mkdir -p /root/envs

# Prepare main virtual environment (for language server features).
COPY ./env/ /root/
RUN cd /root && python3 -m venv .env
RUN /root/.env/bin/pip install -r /root/requirements.txt

# Install Neovim COC extensions.
RUN cd /root/.config/coc/extensions && npm install $COC --global --only=prod

# Install debugger adapter.
RUN cd /root/.config/nvim/plugins/vimspector && python3 install_gadget.py --force-enable-node --enable-go --enable-python && go install github.com/go-delve/delve/cmd/dlv@latest

# Copy Neovim configuration files.
COPY ./config/ /root/.config/nvim/

# Avoid container exit.
CMD ["tail", "-f", "/dev/null"]
