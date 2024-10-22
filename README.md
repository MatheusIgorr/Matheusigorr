# Nome da Action
name: Snake Game

# Controlador do tempo que será feita a atualização dos arquivos
on:
  schedule:
    # Será atualizado a cada 5 horas
    - cron: "0 */5 * * *"

  # Permite executar manualmente na lista de Actions (utilizado para testes de build)
  workflow_dispatch:

# Regras
jobs:
  build:
    runs-on: ubuntu-latest
    steps:

    # Verifica o repositório, para que seu job possa acessá-lo
      - uses: actions/checkout@v2

    # Repositório que será utilizado para gerar os arquivos
      - uses: Platane/snk@master
        id: snake-gif
        with:
          github_user_name: nomeUsuario # Substitua 'nomeUsuario' pelo seu nome de usuário do GitHub
          gif_out_path: dist/github-contribution-grid-snake.gif
          svg_out_path: dist/github-contribution-grid-snake.svg

    # Exibe o status do git
      - run: git status

    # Para enviar as atualizações
      - name: Push changes
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          branch: master
          force: true

    # Publica os arquivos na branch 'output'
      - uses: crazy-max/ghaction-github-pages@v2.1.3
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

