Paper No:1
Paper/Article title: Mutation Testing via Iterative Large Language Model-Driven Scientific Debugging
Link: https://arxiv.org/abs/2503.08182 
Conference/Journal name:arXiv
Summary of the paper: This paper evaluates whether applying an LLM loop where the process is as follows - hypothesis formulation, experimentation, and conclusion - is efficient at generating unit tests for killing code mutants. Code mutants are deliberately modified versions of a program's source code used in software testing to check if a test suite can spot errors. The researchers behind this project compare several methods of experimentation: single prompt baselines, iterative-prompting, Scientific debugging prompting, and Pynguin (a state-of-the-art search-based test generation tool) across a sample body of 10 Python projects. The results show that iterative prompting achieves roughly 80% in branch coverage and mutation scores as opposed to Pynguin, though the former has larger token and execution costs.
 
How is it relevant to our project?: Our end goal is to build a tool that plants small, realistic errors in the doc on purpose and proves each one is genuinely wrong by finding a concrete input where the code and doc disagree. This paper attacks the verification part of the problem, in that given a produced error/defect, an automated agent can generate an input that exploits the behavioral discrepancy and fails on the mutant while passing the original.

Can you think of some parts/extensions of the paper that we can use in your project?: Here are some things we can adopt from the paper in our own research and implementation. When building out and running our tool, we can do so in a container through a service such as Docker. Along with this, some other things I took note of were a set runtime so that we do not have our tool running longer than expected, and execution error parsing to safely execute LLM-generated inputs against Python/Java target code without side effects.
Overall comment: This paper can offer insight in building the tool in the sense that we can use it to identify mutants and generate the tests to either pass or fail them. The only problem I have with this is that we will have to incur token costs, though using an open source model rather than something from the frontier labs can be more affordable and appropriate for our use case since there isn’t a necessity for significant reasoning capabilities.

Paper No:2
Paper/Article title: Boosting LLMs for Mutation Generation
Link: https://arxiv.org/abs/2603.24560v1 
Conference/Journal name:arXiv
Summary of the paper: This paper introduces a framework that is designed to overcome limitations in existing LLM-based mutation testing (such as low generation rates, uncompilable code, duplicate mutants, and lack of semantic resemblance to real bugs).
SMART combines three core innovations:
Logic-based code chunking that splits target methods along control-flow and declaration boundaries 
Retrieval-Augmented Generation (RAG) using CodeT5 embeddings and Euclidean distance across a vectorized dataset of 130,000 real bug–fix pairs to inject context-aware few-shot examples 
Supervised Fine-Tuning (SFT) on 13,760 mutations coupled with real-world defect
Evaluated on 1,991 Java bugs from Defects4J and ConDefects, SMART achieves a 65.6% generation rate (vs. 42.89% baseline),  90.21% compilability rate, 92.61% real-bug detection rate 


How is it relevant to our project?: I find this relevant to our project, because our tool will plant purposeful errors within the documentation. Naive AI prompts will often generate trivial and unrealistic mutations, as I believe we are evaluating documentation based errors that are more logical and high-level, rather than simple typos and minute details. 


Can you think of some parts/extensions of the paper that we can use in your project?: I think that we can use the RAG pipeline approach that SMART utilized, because we will have a vectorized database of real commits in which developers fixed documentation. When planting an error in a target docstring, we can retrieve similar historical doc bugs to prompt the LLM to generate realistic doc mutations (e.g., swapping row/column orientation, swapping parameter types, reversing boolean conditions) 


Overall comment: I think this paper is helpful in giving us a point of reference when building the main engine of our tool so that our planted errors are actually valid. The only thing I believe we should be wary of is ensuring that the errors that we test against are up to our standard, though we can enforce that later on. 

Paper No:3 
Paper/Article title: Not as Sweet by Another Name: An Empirical Study of Format Robustness in LLM Document Workflows
Link: https://arxiv.org/abs/2607.27648v1 
Conference/Journal name:arXiv/IEEE/ACM 
Summary of the paper: This paper evaluates the robustness of the LLM-document pipeline, testing model behavior across multiple different formats (txt, csv, json, md). The author’s behind the research paper conducted and assessed their experiments against the following metrics: decision outcome, reasoning evidence, and execution stability. Across a body of 48,000 executions that were derived from 4 LLM workstreams (Anthropic, Google, OpenAI, Qwen), the findings demonstrate that simply altering the file encoding leads to accuracy drops reaching 56.00% while inducing conflicting decision outcomes across more than 41% of cases. Organized structures like CSV and JSON routinely scrambled model focus, triggering hidden failures where underlying logical evidence drastically alters despite static end choices. To address this, the researchers recommend a lean format-routing technique that translates inputs into their ideal state prior to execution, cutting structural defects by nearly 44.21% without requiring model fine-tuning.

How is it relevant to our project?: This paper supports our Javadoc research by proving that changing a document's layout completely alters how an AI processes information. The authors found that complex formatting distracts the AI from the actual content. Similarly, altering the structure of our Javadocs could confuse the model, making our formal claim conversions unreliable.

The paper highlights that JSON formatting can confuse AI models and make their reasoning unstable. This directly impacts our coding tutor platform, which relies on strict JSON schemas to guide the AI's logic and track ELO scores. To account for this instability and accurately collect evidence of the model's reasoning, we are analyzing Markdown files and commit history rather than relying purely on the JSON outputs 

Can you think of some parts/extensions of the paper that we can use in your project?: We can use the paper's testing methods to evaluate how different provider workflows (OpenAI, Anthropic, Google, Alibaba) handle our Javadoc mutations. Because each workflow uses different internal mechanisms to upload, parse, and organize files before the model even reads them, structural changes will impact each system differently. We can test our structurally mutated docstrings across these specific workflows to determine which provider's parsing mechanism offers the most reliable inconsistency detection
