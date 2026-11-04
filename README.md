# cicd-demo

Commit with `feat:` / `fix:` → release-please opens a release PR → merge it → image pushed to Docker Hub → deployed on the server.

## Setup

1. **Server** (Ubuntu, see [PROVIDERS.md](PROVIDERS.md)): `curl -fsSL https://get.docker.com | sh && sudo usermod -aG docker $USER`, then log out and back in.
2. **SSH key for CI**: `ssh-keygen -t ed25519 -f deploy_key -N ""`, append `deploy_key.pub` to `~/.ssh/authorized_keys` on the server.
3. **Docker Hub**: create an access token (Account settings → Personal access tokens).
4. **GitHub repo secrets**: `DOCKERHUB_USERNAME`, `DOCKERHUB_TOKEN`, `SSH_HOST`, `SSH_USER`, `SSH_KEY` (content of `deploy_key`).
5. **Settings → Actions → General**: tick "Allow GitHub Actions to create and approve pull requests".
6. Push a `feat: ...` commit to `main`, merge the release PR, open `http://<server-ip>`.
