# Instructions for TypeScript/JavaScript String Formatter

## Tasks

### Run Test

    npm run test

### Build

    npm run build:dev
    npm run build:prod

## Profiling
    cd benchmarks
    node --prof ./benchmark.mjs
    node --prof-process ./isolate-*.log > processed.txt

## Publish
    // Update changelog
    git log --pretty="- %s"

    // Update version number
    npm version major|minor|patch

    // Build production version
    npm run build:prod

    // Test package
    npm pack

    // Publish
    npm login
    npm publish --access public

