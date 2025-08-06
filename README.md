# sharp for AWS Lambda Layers

[![GitHub release](https://img.shields.io/github/tag/pH200/sharp-layer.svg)](https://github.com/pH200/sharp-layer/tags)
[![Build action](https://github.com/pH200/sharp-layer/actions/workflows/build.yml/badge.svg)](https://github.com/pH200/sharp-layer/actions/workflows/build.yml)

## About

The prebuilt [sharp](https://www.npmjs.com/package/sharp) node module for AWS Lambda layer.

### Features

- Built and tested automatically using GitHub Actions
- Automatically releases [sharp](https://www.npmjs.com/package/sharp) updates with GitHub Actions
- Separated builds for `arm64` and `x64`
- Minified and bundled with `esbuild`
- Minimum `6.98 MB` zip file to optimize cold start time

### Why use a bundled Lambda function? / Why separate build for arm64?

Please check out [Optimizing Node.js dependencies in AWS Lambda](https://aws.amazon.com/blogs/compute/optimizing-node-js-dependencies-in-aws-lambda/) for details. A bundled and minified lambda function can be up to 70% faster for cold starts. The package size is also crucial for cold start performance.

## Download

[**Releases**](https://github.com/pH200/sharp-layer/releases)

Download latest [release-arm64.zip](https://github.com/pH200/sharp-layer/releases/latest/download/release-arm64.zip) or [release-x64.zip](https://github.com/pH200/sharp-layer/releases/latest/download/release-x64.zip)

## Usage

```js
import sharp from 'sharp'
```

Check out [aws: Creating and sharing Lambda layers](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html) for more details.

This package can be used with [sst v2](https://v2.sst.dev). Check out [docs.sst.dev: Lambda Layers](https://v2.sst.dev/advanced/lambda-layers) and [sst.dev: Resize Images](https://guide.sst.dev/examples/how-to-automatically-resize-images-with-serverless.html) for examples.

For [sst v3](https://sst.dev), you need to set `sharp` in the `nodejs.install` array. [Check out the example](https://sst.dev/docs/examples/#sharp-image-resizer).

### Setting arm64 for sst v2 functions

```js
function: {
  handler: '{handler}',
  runtime: 'nodejs18.x',
  architecture: 'arm_64',
  nodejs: {
    esbuild: {
      external: ['sharp'],
    },
  },
  layers: [
    new lambda.LayerVersion(stack, 'SharpLayer', {
      code: lambda.Code.fromAsset('layers/sharp'),
      compatibleArchitectures: [
        lambda.Architecture.ARM_64
      ]
    }),
  ]
}
```

### Setting up a lambda layer for AWS SAM

Providing **a zip file** locally actually works, even though it's **not** mentioned in the documentation.

```yml
  ## Lambda
  ImageFunction:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: image-lambda/
      Handler: app.handler
      Runtime: nodejs18.x
      Architectures:
        - arm64
      Timeout: 30
      MemorySize: 1024
      Layers:
        - !Ref SharpLayer
    Metadata:
      BuildMethod: esbuild
      BuildProperties:
        # Check these two issues for problems related to esm and esbuild
        # https://github.com/evanw/esbuild/issues/1921
        # https://github.com/evanw/esbuild/pull/2067#issuecomment-1503688128
        # Switch to cjs when esm doesn't work
        Format: esm
        OutExtension:
          - .js=.mjs
        EntryPoints:
          - app.ts
        External:
          - '@aws-sdk/*' # @aws-sdk 3.x is installed globally for nodejs18.x
          - sharp # use layer
  ## Lambda layer
  SharpLayer:
    Type: AWS::Serverless::LayerVersion
    Properties:
      LayerName: sharp
      ContentUri: layers/sharp/release-arm64.zip # zip
      CompatibleArchitectures:
        - arm64
      CompatibleRuntimes:
        - nodejs18.x
        - nodejs16.x
```

## Build

Fork this repo -> Actions -> Run build.yml

## References

[Umkus/lambda-layer-sharp](https://github.com/Umkus/lambda-layer-sharp) - another maintained sharp lambda layer

[aws: Creating and sharing Lambda layers](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html)

[aws: Working with layers](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-layers.html)

[aws: Building layers](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/building-layers.html)

[sharp: Installation - AWS Lambda](https://sharp.pixelplumbing.com/install#aws-lambda)
