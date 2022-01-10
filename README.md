# PL-open-projects

On-going list of open projects

## Encrypt at rest

- Problem: A user who puts data into IPFS expects their data to be unreadable by storage providers. We are being compared to "Encryption at Rest" solutions from other providers - https://cloud.google.com/security/encryption/default-encryption.
- Solution: Provide a system that encrypts data at rest to prevent malicious storage providers from viewing.
  - NTH: Also integrate/provide first class ways to do ACL against data.
- Status:
  - Currently working with LIT protocol () to answer a series of "hard questions" - https://docs.google.com/document/d/1RwP5_BAvNGLknn2k0m0VmdPN8udXjOtVuIHZVI-JSwE/edit#heading=h.t6kjohgl6y44
  - LIT will be submitting a FIP to discuss pluggability of the Lotus

## Compute-over-Data

- Problem: Many data sets need some form of engineering before downloading, use. Further, machines running these data sets are required
- Solution: Provide primitives for "compute over data" enabling execution over datasets on the nodes where the data is being hosted. See much more here (https://github.com/filecoin-project/bacalhau)
- Prior art:
  - https://research.protocol.ai/publications/ipfs-fan-a-function-addressable-computation-network/delarocha2021a.pdf
  - https://arxiv.org/pdf/2101.01901v1.pdf
  - https://github.com/tesserai/iptf
  - https://github.com/yenkuanlee/IPDC
  - Also worth watching Containers at HyperSpeed https://www.youtube.com/watch?v=vaIWRyotz4g

## Checkpointing ML models to IPFS

- Problem: Training models with machine learning is a very large, time consuming project, which does not translate between organizations and/or is non-portable. If we enabled more granular models (that could be composed) or better checkpointing, this could unlock faster model building and customer impact. However, these intermediate artifacts can be very large, and costly to store indefinitely. We need a file format, integrations with common MLOps platforms/frameworks, and an open ended storage endpoint that can CRUD these intermediate artifacts to the world, to enable cross industry collaboration.
- Solution:
  - Build a prototype of a version control system for model checkpoints.
    - As a preliminary plan, the system would operate on an ONNX backend and support various kinds of patches (sparse parameter update, add parameters, low-rank parameter update, updating all parameters...) and merging schemes (parameter averaging like in federated averaging or Fisher merging).
    - Use IPFS as a backend for storage and testing how well IPFS scales with different models and patching strategies.
  - Decomposing tasks into different subtasks, e.g. dog detection requires identifying eyes, noses, ears, etc. and textual entailment requires understanding the meaning of different words, paraphrasing, some commonsense reasoning, etc.
    - Ideally we should be able to take a model trained on some particular task, identify the "skills" it has learned, and extract and recombine those skills into different models (without gradient-based training). We have
    - Identify (not necessarily extract/recombine) these "skills".
    - See if it is valuable to find the minimum collection of datasets that can reliably be used to "unit test" your model, i.e. show that it works sufficiently well on a diverse range of tasks.
  - Making conditional computation/adapter-based/mixture-of-expert models more flexible and performant.
    - Currently models that route their computation through different modules are hard to train and their performance lags behind normal models without conditional computation.
    - The only models for which this is not true are ones where the routing is not performed adaptively, e.g. all examples from a given task are routed through the same module (these are called "task adapters"). 
    - Characterize why this is the case, and are hoping to do some future work on bridging the gap between learned/adaptive routing and hard-coded/heuristic routing.

## Boot/Execute/Serverless from IPFS

- Problem: Spinning up workloads/new machines is complex. Pre-built images are not ideal (may be out of date and/or hard to get going), when common tools (PXE) already exist. Automatically running functionality (scripts, containers, binaries) requires additional pre-baking of the image or automated download. Combining all this functionality together would make for much simpler provisioning of devices / edge execution.
- Solution: Use IPFS as a store of both the image and execution, and use only the minimum amount of information on the device in order to bootstrap, pick up the remainder of the application from IPFS.

## Executable documentation

- Problem: Documentation is almost instantly out of date and detached from the process of building software. This means that there is limited feedback to the development or documentation team about things that may be out of date, breaking changes, or regressions.
- Solution: Build a system that automatically converted documentation into executable tests - this is NOT an NLP problem, just a templating one. The process would:
  - Enable sections that are "hidden" before publication (e.g. tokens, setup scripts etc)
  - Strip all non-code from the existing documentation
  - Have a set of expectations in the hidden sections that would evaluate if the test "worked" or not.

## Kubeflow/Kubernetes/Notebook integration with IPFS

- Problem: Data scientists begin with notebooks and any platform that does not meet them where they are ultimately results in artifacts which are mostly unsuitable for production. Further, even if they wrote the notebook or python script perfectly, the final product may not be executable in a distributed way (e.g. parallelizable, cacheable, etc). Finally, even should all of these be solved, the source of the data is often disconnected, to the point that a common pattern is to take a live data source and dump it to a "warm" location that is static (and under the control of the data scientist) - e.g. S3.
- Solution: Provide native integration of IPFS into data/ML pipelines (using Kubeflow as a first example, but supporting Airflow, Sagemaker, Azure ML and Google Vertex at a minimum). Further, enable transformation and parameterization of these notebooks into production-ready artifacts, further simplifying IPFS's use as a data storage location.
  - See more: https://sameproject.ml/

## Declarative setup of PL Infra

- Problem: Setting up of Lotus nodes on common deployment platforms is too challenging. This results in fewer miners, more complex debugging, and, ultimately, less data available on the network.
- Solution: Provide declarative setup tools (e.g. helm charts) that work against common deployment platforms (e.g. Kubernetes). Integrate this with our current CI/CD systems to allow for more sophisticated integration testing of new lotus releases before rollout.
  - See more: https://github.com/filecoin-project/athena

## Distributed professional networking site

- Problem: Current professional networking sites do not validate credentials, and should support non-standard credentials (e.g. I know how to solder, I took an ML class at Google, etc), that could be validated by organizations. 
- Solution: Use a blockchain as a "public" way to present credentials and enabled "views" into these credentials from a single, permessioned location. Working discussion: https://docs.google.com/document/d/19fpxNKmV1-1oK_Yl81QKLWwLvwUL4JP1FVDORN1TIxg/edit
