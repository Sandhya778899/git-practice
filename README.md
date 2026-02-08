name: Fresh CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Build application
        run: |
          echo "🔧 Building application..."
          mkdir build
          echo "app binary" > build/app.txt

      - name: Upload build artifact
        uses: actions/upload-artifact@v4
        with:
          name: app-build
          path: build/

  test:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Run tests
        run: |
          echo "🧪 Running tests..."
          echo "✅ Tests passed"

  deploy:
    runs-on: ubuntu-latest
    needs: test
    environment: production
    steps:
      - name: Download artifact
        uses: actions/download-artifact@v4
        with:
          name: app-build

      - name: Deploy application
        run: |
          echo "🚀 Deploying to PRODUCTION"
          cat app.txt
