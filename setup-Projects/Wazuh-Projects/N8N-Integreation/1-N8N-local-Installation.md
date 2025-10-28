# N8N Installation in Ubuntu

## Step 1: Install Node.js , npm and nvm

### Download and install nvm:
`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash`

### in lieu of restarting the shell
Using Terminal:
```bash
\. "$HOME/.nvm/nvm.sh"
```
### Download and install Node.js:
`nvm install 24`

### Verify the Node.js version:
`node -v # Should print "v24.11.0".`

### Verify npm version:
`npm -v # Should print "11.6.1".`

## Step 2: Install n8n Globally
Using Terminal:
```bash
sudo npm install -g n8n
```
## Step 3: Start n8n
Using Terminal: `n8n`

**URL :** http://localhost:5678














## Reference: 
[https://mehulgohil.com/blog/install-n8n-locally/](https://nodejs.org/en/download)
