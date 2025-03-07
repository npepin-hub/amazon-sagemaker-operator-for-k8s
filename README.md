# Amazon SageMaker Operators for Kubernetes
![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/aws/amazon-sagemaker-operator-for-k8s?sort=semver&logo=amazon-aws&color=232F3E)
[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg?color=success)](http://www.apache.org/licenses/LICENSE-2.0)
![GitHub go.mod Go version](https://img.shields.io/github/go-mod/go-version/aws/amazon-sagemaker-operator-for-k8s?color=69D7E5)

## Introduction
Amazon SageMaker Operators for Kubernetes are operators that can be used to train machine learning models, optimize hyperparameters for a given model, run batch transform jobs over existing models, and set up inference endpoints. With these operators, users can manage their jobs in Amazon SageMaker from their Kubernetes cluster in Amazon Elastic Kubernetes Service [EKS](http://aws.amazon.com/eks).

## Migrate resources to the new SageMaker Operators for Kubernetes
> :warning: This project will reach its end-of-life (EOL) on **Feb 15, 2023** along with [Amazon Elastic Kubernetes Service Kubernetes 1.21](https://docs.aws.amazon.com/eks/latest/userguide/kubernetes-versions.html#kubernetes-release-calendar). If you are currently using version *v1.2.2* or below of this SageMaker Operators for Kubernetes, we recommend migrating your resources to the latest SageMaker Operators for Kubernetes, the **[ACK service controller for Amazon SageMaker](https://github.com/aws-controllers-k8s/sagemaker-controller)** based on [AWS Controllers for Kubernetes (ACK)](https://aws-controllers-k8s.github.io/community/).   

- For information on the migration steps, see [Migrate resources to the latest Operators.](https://docs.aws.amazon.com/sagemaker/latest/dg/kubernetes-sagemaker-operators-migrate.html).
- For answers to frequently asked questions (FAQ) regarding the end of support of this version of SageMaker Operators for Kubernetes, see [Announcing the End of Support of the Original Version of SageMaker Operator for Kubernetes.](https://docs.aws.amazon.com/sagemaker/latest/dg/kubernetes-sagemaker-operators-eos-announcement.html).


## Usage
> :note: The following steps do not install the latest version of SageMaker Operators for Kubernetes. Find the link to the new ACK-based SageMaker Operators for Kubernetes project in the warning message above.

First, you must [install the operators](https://docs.aws.amazon.com/sagemaker/latest/dg/kubernetes-sagemaker-operators.html). After installation is complete, create a TrainingJob YAML specification by following one of the samples, like [samples/xgboost-mnist-trainingjob.yaml](./samples/xgboost-mnist-trainingjob.yaml). Then, use `kubectl` to create and monitor the progress of your job:

```bash
$ kubectl apply -f xgboost-mnist-trainingjob.yaml
trainingjob.sagemaker.aws.amazon.com/xgboost-mnist created

$ kubectl get trainingjob
NAME            STATUS       SECONDARY-STATUS   CREATION-TIME          SAGEMAKER-JOB-NAME
xgboost-mnist   InProgress   Starting           2019-11-26T23:38:11Z   xgboost-mnist-cf1e16fb10a511eaaa450a350733ba06
```

Once the job starts training, you can use a `kubectl` plugin to stream training logs:

```bash
$ kubectl get trainingjob
NAME            STATUS       SECONDARY-STATUS   CREATION-TIME          SAGEMAKER-JOB-NAME
xgboost-mnist   InProgress   Training           2019-11-26T23:38:11Z   xgboost-mnist-cf1e16fb10a511eaaa450a350733ba06

$ kubectl smlogs trainingjob xgboost-mnist | head -n 5
"xgboost-mnist" has SageMaker TrainingJobName "xgboost-mnist-cf1e16fb10a511eaaa450a350733ba06" in region "us-east-2", status "InProgress" and secondary status "Training"
xgboost-mnist-cf1e16fb10a511eaaa450a350733ba06/algo-1-1574811611 2019-11-26 15:41:13.449 -0800 PST Arguments: train
xgboost-mnist-cf1e16fb10a511eaaa450a350733ba06/algo-1-1574811611 2019-11-26 15:41:13.449 -0800 PST [2019-11-26:23:41:10:INFO] Running standalone xgboost training.
xgboost-mnist-cf1e16fb10a511eaaa450a350733ba06/algo-1-1574811611 2019-11-26 15:41:13.45 -0800 PST [2019-11-26:23:41:10:INFO] File size need to be processed in the node: 1122.95mb. Available memory size in the node: 8501.08mb
xgboost-mnist-cf1e16fb10a511eaaa450a350733ba06/algo-1-1574811611 2019-11-26 15:41:13.45 -0800 PST [2019-11-26:23:41:10:INFO] Determined delimiter of CSV input is ','
xgboost-mnist-cf1e16fb10a511eaaa450a350733ba06/algo-1-1574811611 2019-11-26 15:41:13.45 -0800 PST [23:41:10] S3DistributionType set as FullyReplicated
```

The Amazon SageMaker Operators for Kubernetes enable management of SageMaker TrainingJobs, HyperParameterTuningJobs, BatchTransformJobs and HostingDeployments (Endpoints). Create and monitor them using the same `kubectl` tool as above.

To install the operators onto your Kubernetes cluster, follow our [User Guide](https://docs.aws.amazon.com/sagemaker/latest/dg/kubernetes-sagemaker-operators.html).

### YAML Examples

To make a YAML spec, follow one of the below examples as a guide. Replace values like RoleARN, S3 input buckets and S3 output buckets with values that correspond to your account.

* [BatchTransformJob](./samples/xgboost-mnist-batchtransform.yaml)
* [HostingDeployment (Endpoint)](./samples/xgboost-mnist-hostingdeployment.yaml)
* [HyperParameterTuningJob](./samples/xgboost-mnist-hpo.yaml)
* [TrainingJob](./samples/xgboost-mnist-trainingjob.yaml)

## Releases

Amazon SageMaker Operator for Kubernetes adheres to the [SemVer](https://semver.org/) specification. Each release updates the major version tag (eg. `vX`), a major/minor version tag (eg. `vX.Y`) and a major/minor/patch version tag (eg. `vX.Y.Z`), as well as new versions of the `smlogs` binary with URLs of the same versioning formats. To see a full list of all releases, refer to our [Github releases page](https://github.com/aws/amazon-sagemaker-operator-for-k8s/releases).

We also maintain a `latest` tag, which is updated to stay in line with the `master` branch. We **do not** recommend installing this on any production cluster, as any new major versions updated on the `master` branch will introduce breaking changes.

## Contributing
`amazon-sagemaker-operator-for-k8s` is an open source project. See [CONTRIBUTING](https://github.com/aws/amazon-sagemaker-operator-for-k8s/blob/master/CONTRIBUTING.md) for details.

## License

This project is distributed under the
[Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0),
see [LICENSE](https://github.com/aws/amazon-sagemaker-operator-for-k8s/blob/master/LICENSE) and [NOTICE](https://github.com/aws/amazon-sagemaker-operator-for-k8s/blob/master/NOTICE) for more information.
