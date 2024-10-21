name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
        
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'

      - name: Install dependencies
        run: npm install
        
      - name: Run tests
        run: npm test

      - name: Build the project
        run: npm run build

  security_analysis:
    runs-on: ubuntu-latest
    needs: build  # Зависимость от предыдущей работы

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Run AppScreener
        run: |
          echo "Running AppScreener..."
          curl -sSL https://get.app-screener.com | bash
          appscreener check . 

      - name: Run TruffleHog
        run: |
          echo "Running TruffleHog..."
          pip install truffleHog
          trufflehog --json . > trufflehog_report.json
        continue-on-error: true  # Продолжение выполнения, даже если TruffleHog найдет секреты

      - name: Upload TruffleHog report
        uses: actions/upload-artifact@v2
        with:
          name: trufflehog-report
          path: trufflehog_report.json

  deploy:
    runs-on: ubuntu-latest
    needs: security_analysis
    if: github.ref == 'refs/heads/main'  # Развертывание только из основной ветки

    steps:
      - name: Checkout code
        uses: actions/checkout@v2
        
      - name: Deploy to server
        run: |
          echo "Deploying to server..."
          # Здесь добавьте ваши команды для развертывания, например:
          # scp -r ./build user@yourserver:/path/to/deploy
