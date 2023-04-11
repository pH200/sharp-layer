# sharp for AWS Lambda Layers

[![GitHub release](https://img.shields.io/github/tag/pH200/sharp-layer.svg)](https://github.com/pH200/sharp-layer/tags)
[![Get version action](https://github.com/pH200/sharp-layer/actions/workflows/version.yml/badge.svg)](https://github.com/pH200/sharp-layer/actions/workflows/version.yml)
[![Build action](https://github.com/pH200/sharp-layer/actions/workflows/build.yml/badge.svg)](https://github.com/pH200/sharp-layer/actions/workflows/build.yml)

## About

The prebuilt [sharp](https://www.npmjs.com/package/sharp) npm module for AWS Lambda layer.

### Features

- Built and tested automatically using GitHub Actions
- Automatically releases [sharp](https://www.npmjs.com/package/sharp) updates with GitHub Actions
- Separated builds for `arm64` and `x64`
- Minified and bundled with `esbuild`
- Minimum `6.98 MB` zip file to optimize cold start time

### Why use a bundled Lambda function? / Why separate build for arm64?

Please check out [Optimizing Node.js dependencies in AWS Lambda](https://aws.amazon.com/blogs/compute/optimizing-node-js-dependencies-in-aws-lambda/) for details. A bundled and minified lambda function can be up to 70% faster for cold starts. The package size is also crucial for cold start performance.

## Usage

```js
import sharp from 'sharp'
```

Check out [aws: Creating and sharing Lambda layers](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html) for more details.

This package can be used with [sst](https://sst.dev). Check out [docs.sst.dev: Lambda Layers](https://docs.sst.dev/advanced/lambda-layers) and [sst.dev: Resize Images](https://sst.dev/examples/how-to-automatically-resize-images-with-serverless.html) for examples.

## Build

Fork this repo -> Actions -> Run build.yml

## References

[Umkus/lambda-layer-sharp](https://github.com/Umkus/lambda-layer-sharp) - another maintained sharp lambda layer

[aws: Creating and sharing Lambda layers](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html)

[aws: Working with layers](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-layers.html)

[sharp: AWS Lambda](https://sharp.pixelplumbing.com/install#aws-lambda)
