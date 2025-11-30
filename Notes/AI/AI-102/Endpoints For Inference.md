Inference is the process of applying new input data to a machine learning model or pipeline to generate outputs. While these outputs are typically called "predictions," inference can generate outputs for other machine learning tasks, such as classification and clustering. In Azure Machine Learning, you perform inference by using **endpoints**.

An **endpoint** is a stable and durable URL that can be used to request or invoke a model. You provide the required inputs to the endpoint and receive the outputs. Azure Machine Learning supports standard deployments, online endpoints, and batch endpoints. An endpoint provides:

- A stable and durable URL (such as _endpoint-name.region.inference.ml.azure.com_)
- An authentication mechanism
- An authorization mechanism

