name: Gerar cobrinha das contribuições

on:
  schedule:
    - cron: "0 */12 * * *"   # atualiza a cada 12h
  workflow_dispatch:           # permite rodar manualmente
  push:
    branches: [main]

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - name: Gerar SVGs da cobrinha
        uses: Platane/snk@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-snake.svg
            dist/github-snake-dark.svg?palette=github-dark&color_snake=#00D4AA&color_dots=#161b22,#0e4429,#006d32,#26a641,#39d353

      - name: Publicar na branch output
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
