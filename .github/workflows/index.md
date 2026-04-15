name: Deploy Marp slides
on:
  push:
    branches: [main]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - title: Checkout
        uses: actions/checkout@v4
      - title: Build Marp Slide Deck
        uses: KoharaKazuya/marp-cli-action@v4
        with:
          args: index.md -o index.html
      - title: Deploy to GitHub Pages
        uses: actions/upload-pages-artifact@v3
        with:
          path: .
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - title: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
