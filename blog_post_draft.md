## Best Practices Learnt in Productionizing Machine Learning Models

NewVantage survey shows that 77% of businesses report that "business adoption" of big data and AI initiatives continues to represent a big challenge for business, deriving business values from data science and creating better customer experiences at scale remain elusive. Why is that? Because only through production can a deployed data science solution actually serve customers, and unfortunately, the path to machine learning model production remains difficult for many organizations.

Therefore in this article, we will go over how Toyota Connected designed and implemented our Machine Learning Model production strategy, through collaborations between two teams Mobility and Labs, to deliver world-class experiences for our customers. We wanted to share some of the things we've learnt how to productionize, accelerate and deploy machine learning models, hopefully you will find them useful for you and your organization.


Table of Contents
- Options to implement Machine Learning models
- Jupyter vs Python
- Speed up Python
- Containers vs Serverless
- Results

### 1.Options to implement Machine Learning models

At Toyota Connected, machine learning production is a team activity. Data scientists, data engineers, dev-ops  work closely to understand how the prototype code would fit into existing software architecture. Team members in this cross-functional squad may have a completely different stack of programming languages, divergent comprehension of machine learning, architectures, etc to lay the groundwork for collaborations, there are usually the following options that organization pursue.

The first option is rewriting the whole model in the language that the software engineering folks work.
While this approach is great for getting started, the time and energy required to get those intricate models replicated would be significant. Majority of languages like Java, a popular software development programming among engineers and dev-ops, compared to Python which is data scientist's toolkit, do not have great libraries to perform data manipulation and ML training, and that might cause the model outcomes to change.

The second option which we adopted in Toyota Connected is API approach, web APIs have made it easy for cross-language and framework applications to work well. If software developers needs integrate the engineering pipelines with our ML Model, they would just need to get the URL Endpoint from where the API is being served.

By building a REST API for machine learning model,Data scientist could keep model code separate from remaining code bases. It forms a clear division of labor here which is nice for defining responsibilities and prevents anyone from directly blocking teammates who are not involved with the machine learning aspect of the project. Another advantage is that a model can be used by multiple developers working on different platforms, such as web or mobile.We decided to build an API for Machine Learning models.


### 2. Jupyter vs Python

Developing a machine learning model for a new project starts with certain common groundwork and exploration, to understand your data and figure out the approaches to try. A popular choice for data exploration and visualization is Jupyter, a highly interactive, fast-feedback environment, thanks for Jupyter kernels which executes code and retain their internal state while the code is being edited and revised. There are tools such as papermill that is developed to parameterizing and executing Jupyter notebook, and after looking into papermill, it doesn't seem feasible to build a complicated reliable data pipelines.

However, while convenient, Jupyter notebooks have some nonnegligible issues to be used in production. To list a few: Jupyter can be hard to reason about exactly because of hidden states that are retained, it's entirely possible to have a saved notebook that can't be successfully evaluated after relaunching the kernel. Jupyter has no IDE integration or linting meaning we can write awfully bad styled code.Jupyter is very hard to test and structure code reasonable, for developers who are fans of test-driven development, this can be a nightmare.These are some of the reasons why we advocate for using regular, linear Python scripts instead of Jupyter/IPython notebooks when going from initial exploration work to something resembling production.

In short, notebooks break a lot of software engineering paradigms, because they were never meant to be used in software engineering. They were meant for prototyping and interactive experimentation, and thus as soon as we see the need to move to production, we dumped notebook and started refactoring the code into .py modules.


### 3. Speed up Python

We constantly look for ways to optimize our machine learning code, decrease execution time, and ensure that our programs respond as quickly as possible to our customers. Python is our machine learning development language and we would like to share our development practices to optimize Python efficiency and response time to produce minimal latency for our customer experiences using our applications.

Python is known to have an easy syntax that makes learning much more tolerable, and the simplicity and elegance of its syntax really can speed up data scientist's development time multifold, however it’s no secret that the standard Python isn’t as fast at runtime as compiled languages like C or Java, nonetheless that doesn’t mean it should be dismissed for being “too slow.” For the most part, CPU speed is not the be-all-end-all of software development. In fact, these days “developer time” is much more crucial. It would not be wise to throw Python out because we can't improve response times by 10 ms, especially when coding in Python really accelerates the data science development and release cycle.

There’s a couple of points we learnt and follow when looking to speed Python up:

- Vectorization
 Vectorized operations in Numpy are mapped to highly optimized C code, making them much faster than their standard Python or compiled language counterparts like Java. We used Numpy to speed up the processing and prediction operations of our machine learning model by hundreds fold.
  - If there’s a for-loop over an array, there’s a good chance we can replace it with some built-in Numpy function
  - If we see any type of math, there’s a good chance we can replace it with some built-in Numpy function
Both of these points are really focused on replace non-vectorized Python code with optimized, vectorized, low-level C code.

- Use builtin functions and libraries
Builtin functions like sum, max, any, map, etc are implemented in C. They are very efficient and well tested
Guido's Python Patterns - An Optimization Anecdote is a great read:

If you feel the need for speed, go for built-in functions - you can't beat a loop written in C. Check the library manual for a built-in function that does what you want.

-  Keep Python code small and light

It is often the case in programming that simplest is fastest. In this day and age performance is critical and it’s especially important to keep your Python code as compact as possible to reduce latency and speed things up. One article provides some organizing questions that are worth asking in the development stage: “Do we really need this module?”, “Why are we using this framework? Is it worth the overhead?”, “Can we do this in a simpler way?”

With Python’s readability meant that we are able to zero in on the bottleneck in their code in no time, and provide faster response to our customers in C runtime speed using optimized Python Numpy implementations.

### 4. Containers vs Serverless

Serverless/FaaS(Function as a Service) computing and containers are both architectures that reduce overhead for cloud-hosted web applications and more flexibility than applications hosted on traditional servers or virtual machines, they both make it very easy to build API for machine learning models, but they differ in several important ways.

In order to better advise our team in that respect, we have set up a small and dedicated research spike to experiment. To begin, we have established a roadmap for learning the requirements and caveats of deploying machine
learning models into multiple ML pipelines and infrastructures.  One of the biggest challenges is that serving a model(accepting requests and returning predictions) is only part of the problem. There is a long list of adjacent requirements. These include, for example:

- Preprocessing input before predictions
Applications that perform analysis of very large datasets won’t be a good fit for a serverless architecture, as serverless functions have time limits (typically five minutes) before they are terminated.

- Scalability
Serverless allows your app to be elastic. It can automatically scale up to accommodate many concurrent users and scale back down when traffic subsides. This characteristic increases the performance of your app while saving you money.

- Testing
It is difficult to test Serverless web applications because the backend environment is hard to replicate on a local environment. In contrast, containers run the same no matter where they are deployed, making it relatively simple to test a container-based application before deploying it to production.

- Latency
Because servers sit cold until they’re pinged by an application, there is some latency involved in executing tasks. Thus, serverless may not be an ideal solution for applications where speed is paramount, such as e-commerce and search sites.

### 5. Results
TBD


