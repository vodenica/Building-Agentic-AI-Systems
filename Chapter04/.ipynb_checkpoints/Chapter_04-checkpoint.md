<a target="_blank" href="https://colab.research.google.com/github/PacktPublishing/Building-Agentic-AI-Systems/blob/main/Chapter_04.ipynb">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

# Chapter 4 – Reflection and Introspection in Agents
---

Install dependencies


```python
!pip install -U openai ipywidgets crewai pysqlite3-binary
```

    Defaulting to user installation because normal site-packages is not writeable
    Requirement already satisfied: openai in /home/vodenizza/.local/lib/python3.11/site-packages (1.106.1)
    Requirement already satisfied: ipywidgets in /home/vodenizza/.local/lib/python3.11/site-packages (8.1.7)
    Collecting crewai
      Downloading crewai-0.177.0-py3-none-any.whl.metadata (35 kB)
    Collecting pysqlite3-binary
      Downloading pysqlite3_binary-0.5.4-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (766 bytes)
    Requirement already satisfied: anyio<5,>=3.5.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from openai) (4.9.0)
    Requirement already satisfied: distro<2,>=1.7.0 in /usr/lib/python3/dist-packages (from openai) (1.7.0)
    Requirement already satisfied: httpx<1,>=0.23.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from openai) (0.28.1)
    Requirement already satisfied: jiter<1,>=0.4.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from openai) (0.10.0)
    Requirement already satisfied: pydantic<3,>=1.9.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from openai) (2.11.7)
    Requirement already satisfied: sniffio in /home/vodenizza/.local/lib/python3.11/site-packages (from openai) (1.3.1)
    Requirement already satisfied: tqdm>4 in /home/vodenizza/.local/lib/python3.11/site-packages (from openai) (4.67.1)
    Requirement already satisfied: typing-extensions<5,>=4.11 in /home/vodenizza/.local/lib/python3.11/site-packages (from openai) (4.14.1)
    Requirement already satisfied: idna>=2.8 in /usr/lib/python3/dist-packages (from anyio<5,>=3.5.0->openai) (3.3)
    Requirement already satisfied: certifi in /usr/lib/python3/dist-packages (from httpx<1,>=0.23.0->openai) (2020.6.20)
    Requirement already satisfied: httpcore==1.* in /home/vodenizza/.local/lib/python3.11/site-packages (from httpx<1,>=0.23.0->openai) (1.0.9)
    Requirement already satisfied: h11>=0.16 in /home/vodenizza/.local/lib/python3.11/site-packages (from httpcore==1.*->httpx<1,>=0.23.0->openai) (0.16.0)
    Requirement already satisfied: annotated-types>=0.6.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from pydantic<3,>=1.9.0->openai) (0.7.0)
    Requirement already satisfied: pydantic-core==2.33.2 in /home/vodenizza/.local/lib/python3.11/site-packages (from pydantic<3,>=1.9.0->openai) (2.33.2)
    Requirement already satisfied: typing-inspection>=0.4.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from pydantic<3,>=1.9.0->openai) (0.4.1)
    Requirement already satisfied: comm>=0.1.3 in /home/vodenizza/.local/lib/python3.11/site-packages (from ipywidgets) (0.2.3)
    Requirement already satisfied: ipython>=6.1.0 in /usr/lib/python3/dist-packages (from ipywidgets) (7.31.1)
    Requirement already satisfied: traitlets>=4.3.1 in /home/vodenizza/.local/lib/python3.11/site-packages (from ipywidgets) (5.14.3)
    Requirement already satisfied: widgetsnbextension~=4.0.14 in /home/vodenizza/.local/lib/python3.11/site-packages (from ipywidgets) (4.0.14)
    Requirement already satisfied: jupyterlab_widgets~=3.0.15 in /home/vodenizza/.local/lib/python3.11/site-packages (from ipywidgets) (3.0.15)
    Requirement already satisfied: appdirs>=1.4.4 in /usr/lib/python3/dist-packages (from crewai) (1.4.4)
    Collecting blinker>=1.9.0 (from crewai)
      Using cached blinker-1.9.0-py3-none-any.whl.metadata (1.6 kB)
    Requirement already satisfied: chromadb>=0.5.23 in /home/vodenizza/.local/lib/python3.11/site-packages (from crewai) (0.6.3)
    Requirement already satisfied: click>=8.1.7 in /home/vodenizza/.local/lib/python3.11/site-packages (from crewai) (8.2.1)
    Collecting instructor>=1.3.3 (from crewai)
      Downloading instructor-1.11.2-py3-none-any.whl.metadata (11 kB)
    Collecting json-repair==0.25.2 (from crewai)
      Downloading json_repair-0.25.2-py3-none-any.whl.metadata (7.9 kB)
    Requirement already satisfied: json5>=0.10.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from crewai) (0.12.0)
    Requirement already satisfied: jsonref>=1.1.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from crewai) (1.1.0)
    Collecting litellm==1.74.9 (from crewai)
      Downloading litellm-1.74.9-py3-none-any.whl.metadata (40 kB)
    Collecting onnxruntime==1.22.0 (from crewai)
      Downloading onnxruntime-1.22.0-cp311-cp311-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl.metadata (4.5 kB)
    Requirement already satisfied: openpyxl>=3.1.5 in /home/vodenizza/.local/lib/python3.11/site-packages (from crewai) (3.1.5)
    Collecting opentelemetry-api>=1.30.0 (from crewai)
      Using cached opentelemetry_api-1.36.0-py3-none-any.whl.metadata (1.5 kB)
    Collecting opentelemetry-exporter-otlp-proto-http>=1.30.0 (from crewai)
      Downloading opentelemetry_exporter_otlp_proto_http-1.36.0-py3-none-any.whl.metadata (2.3 kB)
    Collecting opentelemetry-sdk>=1.30.0 (from crewai)
      Using cached opentelemetry_sdk-1.36.0-py3-none-any.whl.metadata (1.5 kB)
    Requirement already satisfied: pdfplumber>=0.11.4 in /home/vodenizza/.local/lib/python3.11/site-packages (from crewai) (0.11.7)
    Collecting portalocker==2.7.0 (from crewai)
      Downloading portalocker-2.7.0-py2.py3-none-any.whl.metadata (6.8 kB)
    Requirement already satisfied: pyjwt>=2.9.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from crewai) (2.10.1)
    Requirement already satisfied: python-dotenv>=1.0.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from crewai) (1.1.1)
    Collecting pyvis>=0.3.2 (from crewai)
      Downloading pyvis-0.3.2-py3-none-any.whl.metadata (1.7 kB)
    Requirement already satisfied: regex>=2024.9.11 in /home/vodenizza/.local/lib/python3.11/site-packages (from crewai) (2025.7.34)
    Requirement already satisfied: tokenizers>=0.20.3 in /home/vodenizza/.local/lib/python3.11/site-packages (from crewai) (0.21.4)
    Collecting tomli-w>=1.1.0 (from crewai)
      Downloading tomli_w-1.2.0-py3-none-any.whl.metadata (5.7 kB)
    Collecting tomli>=2.0.2 (from crewai)
      Downloading tomli-2.2.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (11 kB)
    Collecting uv>=0.4.25 (from crewai)
      Downloading uv-0.8.15-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (11 kB)
    Requirement already satisfied: aiohttp>=3.10 in /home/vodenizza/.local/lib/python3.11/site-packages (from litellm==1.74.9->crewai) (3.12.15)
    Requirement already satisfied: importlib-metadata>=6.8.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from litellm==1.74.9->crewai) (8.4.0)
    Requirement already satisfied: jinja2<4.0.0,>=3.1.2 in /home/vodenizza/.local/lib/python3.11/site-packages (from litellm==1.74.9->crewai) (3.1.6)
    Requirement already satisfied: jsonschema<5.0.0,>=4.22.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from litellm==1.74.9->crewai) (4.25.0)
    Requirement already satisfied: tiktoken>=0.7.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from litellm==1.74.9->crewai) (0.9.0)
    Requirement already satisfied: coloredlogs in /home/vodenizza/.local/lib/python3.11/site-packages (from onnxruntime==1.22.0->crewai) (15.0.1)
    Requirement already satisfied: flatbuffers in /home/vodenizza/.local/lib/python3.11/site-packages (from onnxruntime==1.22.0->crewai) (25.2.10)
    Requirement already satisfied: numpy>=1.21.6 in /home/vodenizza/.local/lib/python3.11/site-packages (from onnxruntime==1.22.0->crewai) (1.26.4)
    Requirement already satisfied: packaging in /home/vodenizza/.local/lib/python3.11/site-packages (from onnxruntime==1.22.0->crewai) (24.2)
    Requirement already satisfied: protobuf in /home/vodenizza/.local/lib/python3.11/site-packages (from onnxruntime==1.22.0->crewai) (4.25.8)
    Requirement already satisfied: sympy in /home/vodenizza/.local/lib/python3.11/site-packages (from onnxruntime==1.22.0->crewai) (1.14.0)
    Requirement already satisfied: MarkupSafe>=2.0 in /usr/lib/python3/dist-packages (from jinja2<4.0.0,>=3.1.2->litellm==1.74.9->crewai) (2.0.1)
    Requirement already satisfied: attrs>=22.2.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from jsonschema<5.0.0,>=4.22.0->litellm==1.74.9->crewai) (25.3.0)
    Requirement already satisfied: jsonschema-specifications>=2023.03.6 in /home/vodenizza/.local/lib/python3.11/site-packages (from jsonschema<5.0.0,>=4.22.0->litellm==1.74.9->crewai) (2025.4.1)
    Requirement already satisfied: referencing>=0.28.4 in /home/vodenizza/.local/lib/python3.11/site-packages (from jsonschema<5.0.0,>=4.22.0->litellm==1.74.9->crewai) (0.36.2)
    Requirement already satisfied: rpds-py>=0.7.1 in /home/vodenizza/.local/lib/python3.11/site-packages (from jsonschema<5.0.0,>=4.22.0->litellm==1.74.9->crewai) (0.26.0)
    Requirement already satisfied: aiohappyeyeballs>=2.5.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from aiohttp>=3.10->litellm==1.74.9->crewai) (2.6.1)
    Requirement already satisfied: aiosignal>=1.4.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from aiohttp>=3.10->litellm==1.74.9->crewai) (1.4.0)
    Requirement already satisfied: frozenlist>=1.1.1 in /home/vodenizza/.local/lib/python3.11/site-packages (from aiohttp>=3.10->litellm==1.74.9->crewai) (1.7.0)
    Requirement already satisfied: multidict<7.0,>=4.5 in /home/vodenizza/.local/lib/python3.11/site-packages (from aiohttp>=3.10->litellm==1.74.9->crewai) (6.6.3)
    Requirement already satisfied: propcache>=0.2.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from aiohttp>=3.10->litellm==1.74.9->crewai) (0.3.2)
    Requirement already satisfied: yarl<2.0,>=1.17.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from aiohttp>=3.10->litellm==1.74.9->crewai) (1.20.1)
    Requirement already satisfied: build>=1.0.3 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (1.3.0)
    Requirement already satisfied: chroma-hnswlib==0.7.6 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (0.7.6)
    Requirement already satisfied: fastapi>=0.95.2 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (0.115.14)
    Requirement already satisfied: uvicorn>=0.18.3 in /home/vodenizza/.local/lib/python3.11/site-packages (from uvicorn[standard]>=0.18.3->chromadb>=0.5.23->crewai) (0.29.0)
    Requirement already satisfied: posthog>=2.4.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (6.3.2)
    Requirement already satisfied: opentelemetry-exporter-otlp-proto-grpc>=1.2.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (1.27.0)
    Requirement already satisfied: opentelemetry-instrumentation-fastapi>=0.41b0 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (0.48b0)
    Requirement already satisfied: pypika>=0.48.9 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (0.48.9)
    Requirement already satisfied: overrides>=7.3.1 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (7.7.0)
    Requirement already satisfied: importlib-resources in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (6.5.2)
    Requirement already satisfied: grpcio>=1.58.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (1.74.0)
    Requirement already satisfied: bcrypt>=4.0.1 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (4.3.0)
    Requirement already satisfied: typer>=0.9.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (0.16.0)
    Requirement already satisfied: kubernetes>=28.1.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (33.1.0)
    Requirement already satisfied: tenacity>=8.2.3 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (8.5.0)
    Requirement already satisfied: PyYAML>=6.0.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (6.0.2)
    Requirement already satisfied: mmh3>=4.0.1 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (5.2.0)
    Requirement already satisfied: orjson>=3.9.12 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (3.11.1)
    Requirement already satisfied: rich>=10.11.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from chromadb>=0.5.23->crewai) (13.9.4)
    Requirement already satisfied: pyproject_hooks in /home/vodenizza/.local/lib/python3.11/site-packages (from build>=1.0.3->chromadb>=0.5.23->crewai) (1.2.0)
    Requirement already satisfied: starlette<0.47.0,>=0.40.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from fastapi>=0.95.2->chromadb>=0.5.23->crewai) (0.46.2)
    Requirement already satisfied: zipp>=0.5 in /home/vodenizza/.local/lib/python3.11/site-packages (from importlib-metadata>=6.8.0->litellm==1.74.9->crewai) (3.23.0)
    Collecting diskcache>=5.6.3 (from instructor>=1.3.3->crewai)
      Using cached diskcache-5.6.3-py3-none-any.whl.metadata (20 kB)
    Requirement already satisfied: docstring-parser<1.0,>=0.16 in /home/vodenizza/.local/lib/python3.11/site-packages (from instructor>=1.3.3->crewai) (0.17.0)
    Requirement already satisfied: requests<3.0.0,>=2.32.3 in /home/vodenizza/.local/lib/python3.11/site-packages (from instructor>=1.3.3->crewai) (2.32.4)
    Requirement already satisfied: charset_normalizer<4,>=2 in /home/vodenizza/.local/lib/python3.11/site-packages (from requests<3.0.0,>=2.32.3->instructor>=1.3.3->crewai) (3.4.2)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /home/vodenizza/.local/lib/python3.11/site-packages (from requests<3.0.0,>=2.32.3->instructor>=1.3.3->crewai) (2.5.0)
    Requirement already satisfied: markdown-it-py>=2.2.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from rich>=10.11.0->chromadb>=0.5.23->crewai) (3.0.0)
    Requirement already satisfied: pygments<3.0.0,>=2.13.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from rich>=10.11.0->chromadb>=0.5.23->crewai) (2.19.2)
    Requirement already satisfied: shellingham>=1.3.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from typer>=0.9.0->chromadb>=0.5.23->crewai) (1.5.4)
    Requirement already satisfied: six>=1.9.0 in /usr/lib/python3/dist-packages (from kubernetes>=28.1.0->chromadb>=0.5.23->crewai) (1.16.0)
    Requirement already satisfied: python-dateutil>=2.5.3 in /home/vodenizza/.local/lib/python3.11/site-packages (from kubernetes>=28.1.0->chromadb>=0.5.23->crewai) (2.9.0.post0)
    Requirement already satisfied: google-auth>=1.0.1 in /home/vodenizza/.local/lib/python3.11/site-packages (from kubernetes>=28.1.0->chromadb>=0.5.23->crewai) (2.40.3)
    Requirement already satisfied: websocket-client!=0.40.0,!=0.41.*,!=0.42.*,>=0.32.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from kubernetes>=28.1.0->chromadb>=0.5.23->crewai) (1.8.0)
    Requirement already satisfied: requests-oauthlib in /home/vodenizza/.local/lib/python3.11/site-packages (from kubernetes>=28.1.0->chromadb>=0.5.23->crewai) (2.0.0)
    Requirement already satisfied: oauthlib>=3.2.2 in /home/vodenizza/.local/lib/python3.11/site-packages (from kubernetes>=28.1.0->chromadb>=0.5.23->crewai) (3.3.1)
    Requirement already satisfied: durationpy>=0.7 in /home/vodenizza/.local/lib/python3.11/site-packages (from kubernetes>=28.1.0->chromadb>=0.5.23->crewai) (0.10)
    Requirement already satisfied: cachetools<6.0,>=2.0.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from google-auth>=1.0.1->kubernetes>=28.1.0->chromadb>=0.5.23->crewai) (5.5.2)
    Requirement already satisfied: pyasn1-modules>=0.2.1 in /home/vodenizza/.local/lib/python3.11/site-packages (from google-auth>=1.0.1->kubernetes>=28.1.0->chromadb>=0.5.23->crewai) (0.4.2)
    Requirement already satisfied: rsa<5,>=3.1.4 in /home/vodenizza/.local/lib/python3.11/site-packages (from google-auth>=1.0.1->kubernetes>=28.1.0->chromadb>=0.5.23->crewai) (4.9.1)
    Requirement already satisfied: pyasn1>=0.1.3 in /home/vodenizza/.local/lib/python3.11/site-packages (from rsa<5,>=3.1.4->google-auth>=1.0.1->kubernetes>=28.1.0->chromadb>=0.5.23->crewai) (0.6.1)
    Requirement already satisfied: mdurl~=0.1 in /home/vodenizza/.local/lib/python3.11/site-packages (from markdown-it-py>=2.2.0->rich>=10.11.0->chromadb>=0.5.23->crewai) (0.1.2)
    Requirement already satisfied: et-xmlfile in /home/vodenizza/.local/lib/python3.11/site-packages (from openpyxl>=3.1.5->crewai) (2.0.0)
    Requirement already satisfied: deprecated>=1.2.6 in /home/vodenizza/.local/lib/python3.11/site-packages (from opentelemetry-exporter-otlp-proto-grpc>=1.2.0->chromadb>=0.5.23->crewai) (1.2.18)
    Requirement already satisfied: googleapis-common-protos~=1.52 in /home/vodenizza/.local/lib/python3.11/site-packages (from opentelemetry-exporter-otlp-proto-grpc>=1.2.0->chromadb>=0.5.23->crewai) (1.70.0)
    Requirement already satisfied: opentelemetry-exporter-otlp-proto-common==1.27.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from opentelemetry-exporter-otlp-proto-grpc>=1.2.0->chromadb>=0.5.23->crewai) (1.27.0)
    Requirement already satisfied: opentelemetry-proto==1.27.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from opentelemetry-exporter-otlp-proto-grpc>=1.2.0->chromadb>=0.5.23->crewai) (1.27.0)
    INFO: pip is looking at multiple versions of opentelemetry-exporter-otlp-proto-grpc to determine which version is compatible with other requirements. This could take a while.
    Collecting opentelemetry-exporter-otlp-proto-grpc>=1.2.0 (from chromadb>=0.5.23->crewai)
      Using cached opentelemetry_exporter_otlp_proto_grpc-1.36.0-py3-none-any.whl.metadata (2.4 kB)
    Collecting opentelemetry-exporter-otlp-proto-common==1.36.0 (from opentelemetry-exporter-otlp-proto-grpc>=1.2.0->chromadb>=0.5.23->crewai)
      Using cached opentelemetry_exporter_otlp_proto_common-1.36.0-py3-none-any.whl.metadata (1.8 kB)
    Collecting opentelemetry-proto==1.36.0 (from opentelemetry-exporter-otlp-proto-grpc>=1.2.0->chromadb>=0.5.23->crewai)
      Using cached opentelemetry_proto-1.36.0-py3-none-any.whl.metadata (2.3 kB)
    Collecting protobuf (from onnxruntime==1.22.0->crewai)
      Using cached protobuf-6.32.0-cp39-abi3-manylinux2014_x86_64.whl.metadata (593 bytes)
    Collecting opentelemetry-semantic-conventions==0.57b0 (from opentelemetry-sdk>=1.30.0->crewai)
      Using cached opentelemetry_semantic_conventions-0.57b0-py3-none-any.whl.metadata (2.4 kB)
    Requirement already satisfied: opentelemetry-instrumentation-asgi==0.48b0 in /home/vodenizza/.local/lib/python3.11/site-packages (from opentelemetry-instrumentation-fastapi>=0.41b0->chromadb>=0.5.23->crewai) (0.48b0)
    Requirement already satisfied: opentelemetry-instrumentation==0.48b0 in /home/vodenizza/.local/lib/python3.11/site-packages (from opentelemetry-instrumentation-fastapi>=0.41b0->chromadb>=0.5.23->crewai) (0.48b0)
    INFO: pip is looking at multiple versions of opentelemetry-instrumentation-fastapi to determine which version is compatible with other requirements. This could take a while.
    Collecting opentelemetry-instrumentation-fastapi>=0.41b0 (from chromadb>=0.5.23->crewai)
      Using cached opentelemetry_instrumentation_fastapi-0.57b0-py3-none-any.whl.metadata (2.2 kB)
    Collecting opentelemetry-instrumentation-asgi==0.57b0 (from opentelemetry-instrumentation-fastapi>=0.41b0->chromadb>=0.5.23->crewai)
      Using cached opentelemetry_instrumentation_asgi-0.57b0-py3-none-any.whl.metadata (2.0 kB)
    Collecting opentelemetry-instrumentation==0.57b0 (from opentelemetry-instrumentation-fastapi>=0.41b0->chromadb>=0.5.23->crewai)
      Using cached opentelemetry_instrumentation-0.57b0-py3-none-any.whl.metadata (6.7 kB)
    Collecting opentelemetry-util-http==0.57b0 (from opentelemetry-instrumentation-fastapi>=0.41b0->chromadb>=0.5.23->crewai)
      Downloading opentelemetry_util_http-0.57b0-py3-none-any.whl.metadata (2.6 kB)
    Requirement already satisfied: wrapt<2.0.0,>=1.0.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from opentelemetry-instrumentation==0.57b0->opentelemetry-instrumentation-fastapi>=0.41b0->chromadb>=0.5.23->crewai) (1.17.2)
    Requirement already satisfied: asgiref~=3.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from opentelemetry-instrumentation-asgi==0.57b0->opentelemetry-instrumentation-fastapi>=0.41b0->chromadb>=0.5.23->crewai) (3.9.1)
    Requirement already satisfied: pdfminer.six==20250506 in /home/vodenizza/.local/lib/python3.11/site-packages (from pdfplumber>=0.11.4->crewai) (20250506)
    Requirement already satisfied: Pillow>=9.1 in /home/vodenizza/.local/lib/python3.11/site-packages (from pdfplumber>=0.11.4->crewai) (10.4.0)
    Requirement already satisfied: pypdfium2>=4.18.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from pdfplumber>=0.11.4->crewai) (4.30.0)
    Requirement already satisfied: cryptography>=36.0.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from pdfminer.six==20250506->pdfplumber>=0.11.4->crewai) (43.0.3)
    Requirement already satisfied: cffi>=1.12 in /home/vodenizza/.local/lib/python3.11/site-packages (from cryptography>=36.0.0->pdfminer.six==20250506->pdfplumber>=0.11.4->crewai) (1.17.1)
    Requirement already satisfied: pycparser in /home/vodenizza/.local/lib/python3.11/site-packages (from cffi>=1.12->cryptography>=36.0.0->pdfminer.six==20250506->pdfplumber>=0.11.4->crewai) (2.22)
    Requirement already satisfied: backoff>=1.10.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from posthog>=2.4.0->chromadb>=0.5.23->crewai) (2.2.1)
    Collecting jsonpickle>=1.4.1 (from pyvis>=0.3.2->crewai)
      Downloading jsonpickle-4.1.1-py3-none-any.whl.metadata (8.1 kB)
    Requirement already satisfied: networkx>=1.11 in /home/vodenizza/.local/lib/python3.11/site-packages (from pyvis>=0.3.2->crewai) (3.5)
    Requirement already satisfied: huggingface-hub<1.0,>=0.16.4 in /home/vodenizza/.local/lib/python3.11/site-packages (from tokenizers>=0.20.3->crewai) (0.28.1)
    Requirement already satisfied: filelock in /home/vodenizza/.local/lib/python3.11/site-packages (from huggingface-hub<1.0,>=0.16.4->tokenizers>=0.20.3->crewai) (3.18.0)
    Requirement already satisfied: fsspec>=2023.5.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from huggingface-hub<1.0,>=0.16.4->tokenizers>=0.20.3->crewai) (2024.12.0)
    Requirement already satisfied: httptools>=0.5.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from uvicorn[standard]>=0.18.3->chromadb>=0.5.23->crewai) (0.6.4)
    Requirement already satisfied: uvloop!=0.15.0,!=0.15.1,>=0.14.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from uvicorn[standard]>=0.18.3->chromadb>=0.5.23->crewai) (0.21.0)
    Requirement already satisfied: watchfiles>=0.13 in /home/vodenizza/.local/lib/python3.11/site-packages (from uvicorn[standard]>=0.18.3->chromadb>=0.5.23->crewai) (1.1.0)
    Requirement already satisfied: websockets>=10.4 in /home/vodenizza/.local/lib/python3.11/site-packages (from uvicorn[standard]>=0.18.3->chromadb>=0.5.23->crewai) (13.1)
    Requirement already satisfied: humanfriendly>=9.1 in /home/vodenizza/.local/lib/python3.11/site-packages (from coloredlogs->onnxruntime==1.22.0->crewai) (10.0)
    Requirement already satisfied: mpmath<1.4,>=1.1.0 in /home/vodenizza/.local/lib/python3.11/site-packages (from sympy->onnxruntime==1.22.0->crewai) (1.3.0)
    Downloading crewai-0.177.0-py3-none-any.whl (418 kB)
    Downloading json_repair-0.25.2-py3-none-any.whl (12 kB)
    Downloading litellm-1.74.9-py3-none-any.whl (8.7 MB)
    [2K   [38;2;114;156;31m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m8.7/8.7 MB[0m [31m267.3 kB/s[0m  [33m0:00:34[0m[0m eta [36m0:00:01[0m0:02[0m:04[0m
    [?25hDownloading onnxruntime-1.22.0-cp311-cp311-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl (16.4 MB)
    [2K   [38;2;114;156;31m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m16.4/16.4 MB[0m [31m306.2 kB/s[0m  [33m0:01:00[0m[0m eta [36m0:00:01[0m0:03[0m:04[0m
    [?25hDownloading portalocker-2.7.0-py2.py3-none-any.whl (15 kB)
    Downloading pysqlite3_binary-0.5.4-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (5.2 MB)
    [2K   [38;2;114;156;31m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m5.2/5.2 MB[0m [31m258.3 kB/s[0m  [33m0:00:19[0m7.3 kB/s[0m eta [36m0:00:01[0m
    [?25hUsing cached blinker-1.9.0-py3-none-any.whl (8.5 kB)
    Downloading instructor-1.11.2-py3-none-any.whl (148 kB)
    Using cached diskcache-5.6.3-py3-none-any.whl (45 kB)
    Using cached opentelemetry_api-1.36.0-py3-none-any.whl (65 kB)
    Using cached opentelemetry_exporter_otlp_proto_grpc-1.36.0-py3-none-any.whl (18 kB)
    Using cached opentelemetry_exporter_otlp_proto_common-1.36.0-py3-none-any.whl (18 kB)
    Using cached opentelemetry_proto-1.36.0-py3-none-any.whl (72 kB)
    Using cached opentelemetry_sdk-1.36.0-py3-none-any.whl (119 kB)
    Using cached opentelemetry_semantic_conventions-0.57b0-py3-none-any.whl (201 kB)
    Using cached protobuf-6.32.0-cp39-abi3-manylinux2014_x86_64.whl (322 kB)
    Downloading opentelemetry_exporter_otlp_proto_http-1.36.0-py3-none-any.whl (18 kB)
    Downloading opentelemetry_instrumentation_fastapi-0.57b0-py3-none-any.whl (12 kB)
    Downloading opentelemetry_instrumentation-0.57b0-py3-none-any.whl (32 kB)
    Downloading opentelemetry_instrumentation_asgi-0.57b0-py3-none-any.whl (16 kB)
    Downloading opentelemetry_util_http-0.57b0-py3-none-any.whl (7.6 kB)
    Downloading pyvis-0.3.2-py3-none-any.whl (756 kB)
    [2K   [38;2;114;156;31m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m756.0/756.0 kB[0m [31m324.2 kB/s[0m  [33m0:00:02[0m.2 kB/s[0m eta [36m0:00:01[0m
    [?25hDownloading jsonpickle-4.1.1-py3-none-any.whl (47 kB)
    Downloading tomli-2.2.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (236 kB)
    Downloading tomli_w-1.2.0-py3-none-any.whl (6.7 kB)
    Downloading uv-0.8.15-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (21.0 MB)
    [2K   [38;2;114;156;31m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m21.0/21.0 MB[0m [31m313.9 kB/s[0m  [33m0:01:21[0m[0m eta [36m0:00:01[0m[36m0:00:02[0m
    [?25hInstalling collected packages: pysqlite3-binary, uv, tomli-w, tomli, protobuf, portalocker, opentelemetry-util-http, jsonpickle, json-repair, diskcache, blinker, pyvis, opentelemetry-proto, opentelemetry-api, onnxruntime, opentelemetry-semantic-conventions, opentelemetry-exporter-otlp-proto-common, opentelemetry-sdk, opentelemetry-instrumentation, litellm, instructor, opentelemetry-instrumentation-asgi, opentelemetry-exporter-otlp-proto-http, opentelemetry-exporter-otlp-proto-grpc, opentelemetry-instrumentation-fastapi, crewai
    [2K  Attempting uninstall: protobuf8;2;249;38;114m╸[0m[38;5;237m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m 3/26[0m [tomli]
    [2K    Found existing installation: protobuf 4.25.8[38;5;237m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m 3/26[0m [tomli]
    [2K    Uninstalling protobuf-4.25.8:49;38;114m╸[0m[38;5;237m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m 3/26[0m [tomli]
    [2K      Successfully uninstalled protobuf-4.25.80m[38;5;237m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m 3/26[0m [tomli]
    [2K  Attempting uninstall: opentelemetry-util-httpm[38;5;237m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m 3/26[0m [tomli]
    [2K    Found existing installation: opentelemetry-util-http 0.48b0━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m 3/26[0m [tomli]
    [2K    Uninstalling opentelemetry-util-http-0.48b0:[38;5;237m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m 3/26[0m [tomli]
    [2K      Successfully uninstalled opentelemetry-util-http-0.48b0━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m 3/26[0m [tomli]
    [2K  Attempting uninstall: opentelemetry-proto49;38;114m╸[0m[38;5;237m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m 7/26[0m [jsonpickle]
    [2K    Found existing installation: opentelemetry-proto 1.27.05;237m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m 7/26[0m [jsonpickle]
    [2K    Uninstalling opentelemetry-proto-1.27.0:;114m╸[0m[38;5;237m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m 7/26[0m [jsonpickle]
    [2K      Successfully uninstalled opentelemetry-proto-1.27.08;5;237m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m 7/26[0m [jsonpickle]
    [2K  Attempting uninstall: opentelemetry-api0m[38;5;237m╺[0m[38;5;237m━━━━━━━━━━━━━━━━━━━━━[0m [32m12/26[0m [opentelemetry-proto]
    [2K    Found existing installation: opentelemetry-api 1.27.038;5;237m━━━━━━━━━━━━━━━━━━━━━[0m [32m12/26[0m [opentelemetry-proto]
    [2K    Uninstalling opentelemetry-api-1.27.0:8;5;237m╺[0m[38;5;237m━━━━━━━━━━━━━━━━━━━━━[0m [32m12/26[0m [opentelemetry-proto]
    [2K      Successfully uninstalled opentelemetry-api-1.27.0[38;5;237m━━━━━━━━━━━━━━━━━━━━━[0m [32m12/26[0m [opentelemetry-proto]
    [2K  Attempting uninstall: onnxruntime[0m[38;5;237m╺[0m[38;5;237m━━━━━━━━━━━━━━━━━━━━━[0m [32m12/26[0m [opentelemetry-proto]
    [2K    Found existing installation: onnxruntime 1.22.1[0m[38;5;237m━━━━━━━━━━━━━━━━━━━━━[0m [32m12/26[0m [opentelemetry-proto]
    [2K    Uninstalling onnxruntime-1.22.1:[0m[38;5;237m╺[0m[38;5;237m━━━━━━━━━━━━━━━━━━━━━[0m [32m12/26[0m [opentelemetry-proto]
    [2K      Successfully uninstalled onnxruntime-1.22.1m╺[0m[38;5;237m━━━━━━━━━━━━━━━━━━━━━[0m [32m12/26[0m [opentelemetry-proto]
    [2K  Attempting uninstall: opentelemetry-semantic-conventions8;114m╸[0m[38;5;237m━━━━━━━━━━━━━━━━━━[0m [32m14/26[0m [onnxruntime]
    [2K    Found existing installation: opentelemetry-semantic-conventions 0.48b07m━━━━━━━━━━━━━━━━━━[0m [32m14/26[0m [onnxruntime]
    [2K    Uninstalling opentelemetry-semantic-conventions-0.48b0:m╸[0m[38;5;237m━━━━━━━━━━━━━━━━━━[0m [32m14/26[0m [onnxruntime]
    [2K      Successfully uninstalled opentelemetry-semantic-conventions-0.48b0237m━━━━━━━━━━━━━━━━━━[0m [32m14/26[0m [onnxruntime]
    [2K  Attempting uninstall: opentelemetry-exporter-otlp-proto-common[0m[38;5;237m━━━━━━━━━━━━━━[0m [32m15/26[0m [opentelemetry-semantic-conventions]
    [2K    Found existing installation: opentelemetry-exporter-otlp-proto-common 1.27.0━━━━━━━━[0m [32m15/26[0m [opentelemetry-semantic-conventions]
    [2K    Uninstalling opentelemetry-exporter-otlp-proto-common-1.27.0:38;5;237m━━━━━━━━━━━━━━[0m [32m15/26[0m [opentelemetry-semantic-conventions]
    [2K      Successfully uninstalled opentelemetry-exporter-otlp-proto-common-1.27.0━━━━━━━━━━[0m [32m15/26[0m [opentelemetry-semantic-conventions]
    [2K  Attempting uninstall: opentelemetry-sdk[38;2;249;38;114m╸[0m[38;5;237m━━━━━━━━━━━━━━[0m [32m15/26[0m [opentelemetry-semantic-conventions]
    [2K    Found existing installation: opentelemetry-sdk 1.27.0m╸[0m[38;5;237m━━━━━━━━━━━━━━[0m [32m15/26[0m [opentelemetry-semantic-conventions]
    [2K    Uninstalling opentelemetry-sdk-1.27.0:38;2;249;38;114m╸[0m[38;5;237m━━━━━━━━━━━━━━[0m [32m15/26[0m [opentelemetry-semantic-conventions]
    [2K      Successfully uninstalled opentelemetry-sdk-1.27.014m╸[0m[38;5;237m━━━━━━━━━━━━━━[0m [32m15/26[0m [opentelemetry-semantic-conventions]
    [2K  Attempting uninstall: opentelemetry-instrumentation;114m╸[0m[38;5;237m━━━━━━━━━━━━━━[0m [32m15/26[0m [opentelemetry-semantic-conventions]
    [2K    Found existing installation: opentelemetry-instrumentation 0.48b0;237m━━━━━━━━━━━━━━[0m [32m15/26[0m [opentelemetry-semantic-conventions]
    [2K    Uninstalling opentelemetry-instrumentation-0.48b0:114m╸[0m[38;5;237m━━━━━━━━━━━━━━[0m [32m15/26[0m [opentelemetry-semantic-conventions]
    [2K      Successfully uninstalled opentelemetry-instrumentation-0.48b0;5;237m━━━━━━━━━━━━━━[0m [32m15/26[0m [opentelemetry-semantic-conventions]
    [2K  Attempting uninstall: litellm━━━━━━━━━━━━━━━━━[0m[38;5;237m╺[0m[38;5;237m━━━━━━━━━━━[0m [32m18/26[0m [opentelemetry-instrumentation]ns]
    [2K    Found existing installation: litellm 1.68.0m[38;5;237m╺[0m[38;5;237m━━━━━━━━━━━[0m [32m18/26[0m [opentelemetry-instrumentation]
    [2K    Uninstalling litellm-1.68.0:━━━━━━━━━━━━[0m[38;5;237m╺[0m[38;5;237m━━━━━━━━━━━[0m [32m18/26[0m [opentelemetry-instrumentation]
    [2K      Successfully uninstalled litellm-1.68.0[0m[38;5;237m╺[0m[38;5;237m━━━━━━━━━━━[0m [32m18/26[0m [opentelemetry-instrumentation]
    [2K  Attempting uninstall: opentelemetry-instrumentation-asgi;5;237m╺[0m[38;5;237m━━━━━━━━━━[0m [32m19/26[0m [litellm]-instrumentation]
    [2K    Found existing installation: opentelemetry-instrumentation-asgi 0.48b037m━━━━━━━━━━[0m [32m19/26[0m [litellm]
    [2K    Uninstalling opentelemetry-instrumentation-asgi-0.48b0:7m╺[0m[38;5;237m━━━━━━━━━━[0m [32m19/26[0m [litellm]
    [2K      Successfully uninstalled opentelemetry-instrumentation-asgi-0.48b0;237m━━━━━━━━━━[0m [32m19/26[0m [litellm]
    [2K  Attempting uninstall: opentelemetry-exporter-otlp-proto-grpc[0m[38;5;237m━━━━━━━━━━[0m [32m19/26[0m [litellm]
    [2K    Found existing installation: opentelemetry-exporter-otlp-proto-grpc 1.27.0━━━━━━━━━[0m [32m19/26[0m [litellm]
    [2K    Uninstalling opentelemetry-exporter-otlp-proto-grpc-1.27.0:[0m[38;5;237m━━━━━━━━━━[0m [32m19/26[0m [litellm]
    [2K      Successfully uninstalled opentelemetry-exporter-otlp-proto-grpc-1.27.0m━━━━━━━━━━[0m [32m19/26[0m [litellm]
    [2K  Attempting uninstall: opentelemetry-instrumentation-fastapi;38;114m╸[0m[38;5;237m━━━[0m [32m23/26[0m [opentelemetry-exporter-otlp-proto-grpc]
    [2K    Found existing installation: opentelemetry-instrumentation-fastapi 0.48b0237m━━━[0m [32m23/26[0m [opentelemetry-exporter-otlp-proto-grpc]
    [2K    Uninstalling opentelemetry-instrumentation-fastapi-0.48b0:14m╸[0m[38;5;237m━━━[0m [32m23/26[0m [opentelemetry-exporter-otlp-proto-grpc]
    [2K      Successfully uninstalled opentelemetry-instrumentation-fastapi-0.48b05;237m━━━[0m [32m23/26[0m [opentelemetry-exporter-otlp-proto-grpc]
    [2K   [38;2;114;156;31m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m26/26[0m [crewai]m━[0m [32m25/26[0m [crewai]exporter-otlp-proto-grpc]
    [1A[2K[31mERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
    autogen-core 0.7.1 requires pillow>=11.0.0, but you have pillow 10.4.0 which is incompatible.
    autogen-core 0.7.1 requires protobuf~=5.29.3, but you have protobuf 6.32.0 which is incompatible.
    google-ai-generativelanguage 0.6.6 requires protobuf!=3.20.0,!=3.20.1,!=4.21.0,!=4.21.1,!=4.21.2,!=4.21.3,!=4.21.4,!=4.21.5,<5.0.0dev,>=3.19.5, but you have protobuf 6.32.0 which is incompatible.
    streamlit 1.37.1 requires protobuf<6,>=3.20, but you have protobuf 6.32.0 which is incompatible.[0m[31m
    [0mSuccessfully installed blinker-1.9.0 crewai-0.177.0 diskcache-5.6.3 instructor-1.11.2 json-repair-0.25.2 jsonpickle-4.1.1 litellm-1.74.9 onnxruntime-1.22.0 opentelemetry-api-1.36.0 opentelemetry-exporter-otlp-proto-common-1.36.0 opentelemetry-exporter-otlp-proto-grpc-1.36.0 opentelemetry-exporter-otlp-proto-http-1.36.0 opentelemetry-instrumentation-0.57b0 opentelemetry-instrumentation-asgi-0.57b0 opentelemetry-instrumentation-fastapi-0.57b0 opentelemetry-proto-1.36.0 opentelemetry-sdk-1.36.0 opentelemetry-semantic-conventions-0.57b0 opentelemetry-util-http-0.57b0 portalocker-2.7.0 protobuf-6.32.0 pysqlite3-binary-0.5.4 pyvis-0.3.2 tomli-2.2.1 tomli-w-1.2.0 uv-0.8.15


# 1. Meta Reasoning - example
---

**What is Meta-reassioning?**

**Meta-reasoning** refers to the processes that monitor and control reasoning activities, allowing agents
to reflect upon their own reasoning processes and make adjustments where appropriate. In the context
of a reflective travel agent, meta-reasoning plays a crucial role in enabling the agent to continuously
evaluate and refine its decision-making processes.

Let's take a look at a simple meta-reasoning approach without AI.


```python
import random
```

## Simulated travel agent with meta-reasoning capabilities

- recommend_destination: The agent recommends a destination based on user preferences (budget, luxury, adventure) and internal weightings.

- get_user_feedback: The agent receives feedback on the recommendation (positive or negative).

- meta_reasoning: The agent adjusts its reasoning by updating the weights based on feedback, improving future recommendations.



```python
# Simulated travel agent with meta-reasoning capabilities
class ReflectiveTravelAgent:
    def __init__(self):
        # Initialize preference weights that determine how user preferences influence recommendations
        self.preferences_weights = {
            "budget": 0.5,    # Weight for budget-related preferences
            "luxury": 0.3,    # Weight for luxury-related preferences
            "adventure": 0.2  # Weight for adventure-related preferences
        }
        self.user_feedback = []  # List to store user feedback for meta-reasoning

    def recommend_destination(self, user_preferences):
        """
        Recommend a destination based on user preferences and internal weightings.

        Args:
            user_preferences (dict): User's preferences with keys like 'budget', 'luxury', 'adventure'

        Returns:
            str: Recommended destination
        """
        # Calculate scores for each destination based on weighted user preferences
        score = {
            "Paris": (self.preferences_weights["luxury"] * user_preferences["luxury"] + 
                      self.preferences_weights["adventure"] * user_preferences["adventure"]),
            "Bangkok": (self.preferences_weights["budget"] * user_preferences["budget"] +
                        self.preferences_weights["adventure"] * user_preferences["adventure"]),
            "New York": (self.preferences_weights["luxury"] * user_preferences["luxury"] +
                         self.preferences_weights["budget"] * user_preferences["budget"])
        }
        # Select the destination with the highest calculated score
        recommendation = max(score, key=score.get)
        return recommendation

    def get_user_feedback(self, actual_experience):
        """
        Simulate receiving user feedback and trigger meta-reasoning to adjust recommendations.

        Args:
            actual_experience (str): The destination the user experienced
        """
        # Simulate user feedback: 1 for positive, -1 for negative
        feedback = random.choice([1, -1])
        print(f"Feedback for {actual_experience}: {'Positive' if feedback == 1 else 'Negative'}")
        
        # Store the feedback for later analysis
        self.user_feedback.append((actual_experience, feedback))
        
        # Trigger meta-reasoning to adjust the agent's reasoning process based on feedback
        self.meta_reasoning()

    def meta_reasoning(self):
        """
        Analyze collected feedback and adjust preference weights to improve future recommendations.
        This simulates the agent reflecting on its reasoning process and making adjustments.
        """
        for destination, feedback in self.user_feedback:
            if feedback == -1:  # Negative feedback indicates dissatisfaction
                # Reduce the weight of the main attribute associated with the destination
                if destination == "Paris":
                    self.preferences_weights["luxury"] *= 0.9  # Decrease luxury preference
                elif destination == "Bangkok":
                    self.preferences_weights["budget"] *= 0.9  # Decrease budget preference
                elif destination == "New York":
                    self.preferences_weights["budget"] *= 0.9  # Decrease budget preference
            elif feedback == 1:  # Positive feedback indicates satisfaction
                # Increase the weight of the main attribute associated with the destination
                if destination == "Paris":
                    self.preferences_weights["luxury"] *= 1.1  # Increase luxury preference
                elif destination == "Bangkok":
                    self.preferences_weights["budget"] *= 1.1  # Increase budget preference
                elif destination == "New York":
                    self.preferences_weights["budget"] *= 1.1  # Increase budget preference

        # Normalize weights to ensure they sum up to 1 for consistency
        total_weight = sum(self.preferences_weights.values())
        for key in self.preferences_weights:
            self.preferences_weights[key] /= total_weight

        # Display updated weights after meta-reasoning adjustments
        print(f"Updated weights: {self.preferences_weights}\n")
```

## Simulation

- User Preferences: Defines the user's preferences for budget, luxury, and adventure.

- First Recommendation: The agent recommends a destination based on the initial weights and user preferences.

- User Feedback Simulation: Simulates the user providing feedback on the recommended destination.

- Second Recommendation: After adjusting the weights based on feedback, the agent makes a new recommendation that reflects the updated reasoning process.



```python
# Simulate agent usage
if __name__ == "__main__":
    agent = ReflectiveTravelAgent()

    # User's initial preferences
    user_preferences = {
        "budget": 0.8,      # High preference for budget-friendly options
        "luxury": 0.2,      # Low preference for luxury
        "adventure": 0.5    # Moderate preference for adventure activities
    }

    # First recommendation based on initial preferences and weights
    recommended = agent.recommend_destination(user_preferences)
    print(f"Recommended destination: {recommended}")

    # Simulate user experience and provide feedback
    agent.get_user_feedback(recommended)

    # Second recommendation after adjusting weights based on feedback
    recommended = agent.recommend_destination(user_preferences)
    print(f"Updated recommendation: {recommended}")

```

    Recommended destination: Bangkok
    Feedback for Bangkok: Negative
    Updated weights: {'budget': 0.4736842105263158, 'luxury': 0.3157894736842105, 'adventure': 0.2105263157894737}
    
    Updated recommendation: Bangkok


## Meta-reasoning with AI
---

Now let's bring in AI to perform meta-reasoning with agents. In this case, we will use the `CrewAI` framework to create our meta-reasoning Agents with OpenAI LLMs. We will also emulate user feedback using AI just for demonstration purposes. 

### **What is CrewAI?**

**CrewAI** is a leading platform focused on enabling and orchestrating **multi-agent AI systems**. In this context, "multi-agent" refers to the use of multiple AI agents—each with specialized roles or capabilities—that can collaborate to solve complex tasks more efficiently than a single AI model working alone. CrewAI provides the infrastructure and tools needed to build, deploy, and manage these agentic systems at scale.

A recent partnership between Centrilogic and CrewAI highlights CrewAI's position as a top platform for multi-agent AI, aiming to accelerate the adoption of these technologies in enterprise environments.

---

#### How Can You Use CrewAI with AI Agents?

**Using CrewAI with AI agents typically involves:**

1. **Defining Agent Roles:**  
   You can create multiple AI agents, each with a specific function (e.g., data analysis, customer support, workflow automation).

2. **Orchestrating Collaboration:**  
   CrewAI provides tools to coordinate how these agents interact, share information, and make collective decisions to achieve a common goal.

3. **Integration with Existing Systems:**  
   CrewAI can be integrated into your IT infrastructure, allowing agents to access data, trigger workflows, or interact with users and other software.

4. **Scaling and Managing Agents:**  
   The platform offers management features for monitoring agent performance, scaling up resources as needed, and ensuring security and compliance.

**Example Use Cases:**
- **Enterprise Automation:** Automate complex business processes by assigning different agents to handle document processing, approvals, and notifications.
- **Customer Service:** Deploy a team of agents where one handles FAQs, another escalates complex issues, and a third manages follow-ups.
- **Data Analysis:** Use specialized agents for data collection, cleaning, analysis, and reporting, all coordinated through CrewAI.

---

#### Why Use CrewAI?

- **Efficiency:** Multi-agent systems can break down large tasks and work in parallel, speeding up results.
- **Specialization:** Each agent can be optimized for a specific task, improving accuracy and performance.
- **Scalability:** CrewAI helps manage and scale agentic systems as your needs grow.

---

First, let's make sure we initialize our OpenAI API key, and then let's define the "Crew" (with CrewAI) and the Agents.


```python
import getpass
import os

api_key = getpass.getpass(prompt="Enter OpenAI API Key: ")
os.environ["OPENAI_API_KEY"] = api_key
```

    Enter OpenAI API Key:  ········


We will define **three tools** that our agents will use-

1. `recommend_destination`: This tool will use a set of base weights that prioritize budget, luxury, and adventure equally, and then use the user's preference weights to recommend a destination. Paris will emphasize luxury, NYC emphasizes luxury and adventure, whereas Bangkok emphasizes budget.
2. `update_weights_on_feedback`: This tool will update the internal base weights based on the user's feedback on the recommended destination. Positive feedback will tell the model that its recommendation is correct and it needs to update its internal base weights based on the given (arbitrary adjustment factor), or reduce the weights using the adjustment factor if the feedback is dissatisfied.
3. `feedback_emulator`: This tool will emulate a user providing "satisfied" or "dissatisfied" feedback to the AI agent's destination recommendation


```python
from crewai.tools import tool

# Tool 1
@tool("Recommend travel destination based on preferences.")
def recommend_destination(user_preferences: dict) -> str:
    """
    Recommend a destination based on user preferences and internal weightings.

    Args:
        user_preferences (dict): User's preferences with keys - 'budget', 'luxury', 'adventure'
                                default user_preference weights 'budget' = 0.8, 'luxury' = 0.2, 'adventure' = 0.5
                                user_preferences = {
                                                "budget": 0.8,
                                                "luxury": 0.4,
                                                "adventure": 0.3
                                            }
    Returns:
        str: Recommended destination
    """
    internal_default_weights = {
            "budget": 0.33,    # Weight for budget-related preferences
            "luxury": 0.33,    # Weight for luxury-related preferences
            "adventure": 0.33  # Weight for adventure-related preferences
        }
   # Calculate weighted scores for each destination
    score = {
        "Paris": (
            internal_default_weights["luxury"] * user_preferences["luxury"] +      # Paris emphasizes luxury
            internal_default_weights["adventure"] * user_preferences["adventure"] +
            internal_default_weights["budget"] * user_preferences["budget"]
        ),
        "Bangkok": (
            internal_default_weights["budget"] * user_preferences["budget"] * 2 +  # Bangkok emphasizes budget
            internal_default_weights["luxury"] * user_preferences["luxury"] +
            internal_default_weights["adventure"] * user_preferences["adventure"]
        ),
        "New York": (
            internal_default_weights["luxury"] * user_preferences["luxury"] * 1.5 +  # NYC emphasizes luxury and adventure
            internal_default_weights["adventure"] * user_preferences["adventure"] * 1.5 +
            internal_default_weights["budget"] * user_preferences["budget"]
        )
    }
    
    # Select the destination with the highest calculated score
    recommendation = max(score, key=score.get)
    return recommendation

# Tool 2
@tool("Reasoning tool to adjust preference weights based on user feedback.")
def update_weights_on_feedback(destination: str, feedback: int, adjustment_factor: float) -> dict:
    """
    Analyze collected feedback and adjust internal preference weights based on user feedback for better future recommendations.

    Args:        
        destination (str): The destination recommended ('New York', 'Bangkok' or 'Paris')
        feedback (int): Feedback score; 1 = Satisfied, -1 = dissatisfied
        adjustment_factor (int): The adjustment factor between 0 and 1 that will be used to adjust the internal weights.
                                 Value will be used as (1 - adjustment_factor) for dissatisfied feedback and (1 + adjustment_factor)
                                 for satisfied feedback.
    Returns:
        dict: Adjusted internal weights
    """
    internal_default_weights = {
        "budget": 0.33,    # Weight for budget-related preferences
        "luxury": 0.33,    # Weight for luxury-related preferences
        "adventure": 0.33  # Weight for adventure-related preferences
    }

    # Define primary and secondary characteristics for each destination
    destination_characteristics = {
        "Paris": {
            "primary": "luxury",
            "secondary": "adventure"
        },
        "Bangkok": {
            "primary": "budget",
            "secondary": "adventure"
        },
        "New York": {
            "primary": "luxury",
            "secondary": "adventure"
        }
    }

    # Get the characteristics for the given destination
    dest_chars = destination_characteristics.get(destination, {})
    primary_feature = dest_chars.get("primary")
    secondary_feature = dest_chars.get("secondary")

    # adjustment_factor = 0.2  # How much to adjust weights by

    if feedback == -1:  # Negative feedback
        # Decrease weights for the destination's characteristics
        if primary_feature:
            internal_default_weights[primary_feature] *= (1 - adjustment_factor)
        if secondary_feature:
            internal_default_weights[secondary_feature] *= (1 - adjustment_factor/2)
            
    elif feedback == 1:  # Positive feedback
        # Increase weights for the destination's characteristics
        if primary_feature:
            internal_default_weights[primary_feature] *= (1 + adjustment_factor)
        if secondary_feature:
            internal_default_weights[secondary_feature] *= (1 + adjustment_factor/2)

    # Normalize weights to ensure they sum up to 1
    total_weight = sum(internal_default_weights.values())
    for key in internal_default_weights:
        internal_default_weights[key] = round(internal_default_weights[key] / total_weight, 2)

    # Ensure weights sum to exactly 1.0 after rounding
    adjustment = 1.0 - sum(internal_default_weights.values())
    if adjustment != 0:
        # Add any rounding difference to the largest weight
        max_key = max(internal_default_weights, key=internal_default_weights.get)
        internal_default_weights[max_key] = round(internal_default_weights[max_key] + adjustment, 2)

    return internal_default_weights

# Tool 3
@tool("User feedback emulator tool")
def feedback_emulator(destination: str) -> int:
    """
    Given a destination recommendation (such as 'New York' or 'Bangkok') this tool will emulate to provide
    a user feedback as 1 (satisfied) or -1 (dissatisfied)
    """
    import random
    feedback = random.choice([-1, 1])
    return feedback
```

Once the tools are defined, we will declare **three** `CrewAI` Agents, each of which will use one of the tools above. The `meta_agent` is basically the agent that will perform meta-reasoning using the emulated user feedback and the previously recommended destination to update the internal weights using an `adjustment_factor`. 

Note that here, the model assigns an adjustment factor dynamically to adjust the internal system weights (which is `{"budget": 0.33, "luxury": 0.33, "adventure": 0.33}` in the beginning), i.e., we are not hard-coding the adjustment factor. Although the nature of user feedback in this example is limited to "satisfied" or "dissatisfied" (1 or -1), feedback can be of various forms and may contain more details, in which case your AI Agent may adjust different values to the adjustment_factor. More contextual feedback with details will help the model perform better meta-reasoning on its previous responses.


```python
from crewai import Agent, Task, Crew
from typing import Dict, Union
import random

# Utility functions
def process_recommendation_output(output: str) -> str:
    """Extract the clean destination string from the agent's output."""
    # Handle various ways the agent might format the destination
    for city in ["Paris", "Bangkok", "New York"]:
        if city.lower() in output.lower():
            return city
    return output.strip()

def process_feedback_output(output: Union[Dict, str]) -> int:
    """Extract the feedback value from the agent's output."""
    if isinstance(output, dict):
        return output.get('feedback', 0)
    try:
        # Try to parse as an integer if it's a string
        return int(output)
    except (ValueError, TypeError):
        return 0

def generate_random_preferences():
    # Generate 3 random numbers and normalize them
    values = [random.random() for _ in range(3)]
    total = sum(values)
    
    return {
        "budget": round(values[0]/total, 2),
        "luxury": round(values[1]/total, 2),
        "adventure": round(values[2]/total, 2)
    }

# Initial shared state for weights, preferences, and results
state = {
    "weights": {"budget": 0.33, "luxury": 0.33, "adventure": 0.33},
    "preferences": generate_random_preferences()
}

# Agents Definition
preference_agent = Agent(
    name="Preference Agent",
    role="Travel destination recommender",
    goal="Provide the best travel destination based on user preferences and weights.",
    backstory="An AI travel expert adept at understanding user preferences.",
    verbose=True,
    llm='gpt-4o-mini',
    tools=[recommend_destination]
)

feedback_agent = Agent(
    name="Feedback Agent",
    role="Simulated feedback provider",
    goal="Provide simulated feedback for the recommended travel destination.",
    backstory="An AI that mimics user satisfaction or dissatisfaction for travel recommendations.",
    verbose=True,
    llm='gpt-4o-mini',
    tools=[feedback_emulator]
)

meta_agent = Agent(
    name="Meta-Reasoning Agent",
    role="Preference weight adjuster",
    goal="Reflect on feedback and adjust the preference weights to improve future recommendations.",
    backstory="An AI optimizer that learns from user experiences to fine-tune recommendation preferences.",
    verbose=True,
    llm='gpt-4o-mini',
    tools=[update_weights_on_feedback]
)


# Tasks with data passing that shall be performed by the AI Agents
generate_recommendation = Task(
    name="Generate Recommendation",
    agent=preference_agent,
    description=(
        f"Use the recommend_destination tool with these preferences: {state['preferences']}\n"
        "Return only the destination name as a simple string (Paris, Bangkok, or New York)."
    ),
    expected_output="A destination name as a string",
    output_handler=process_recommendation_output
)

simulate_feedback = Task(
    name="Simulate User Feedback",
    agent=feedback_agent,
    description=(
        "Use the feedback_emulator tool with the destination from the previous task.\n"
        "Instructions:\n"
        "1. Get the destination string from the previous task\n"
        "2. Pass it directly to the feedback_emulator tool\n"
        "3. Return the feedback value (1 or -1)\n\n"
        "IMPORTANT: Pass the destination as a plain string, not a dictionary."
    ),
    expected_output="An integer feedback value: 1 or -1",
    context=[generate_recommendation],
    output_handler=process_feedback_output
)

adjust_weights = Task(
    name="Adjust Weights Based on Feedback",
    agent=meta_agent,
    description=(
        "Use the update_weights_on_feedback tool with:\n"
        "1. destination: Get from first task's output (context[0])\n"
        "2. feedback: Get from second task's output (context[1])\n"
        "3. adjustment_factor: a number between 0 and 1 that will be used to adjust internal weights based on feedback\n\n"
        "Ensure all inputs are in their correct types (string for destination, integer for feedback)."
    ),
    expected_output="Updated weights as a dictionary",
    context=[generate_recommendation, simulate_feedback]
)

# Crew Definition
crew = Crew(
    agents=[preference_agent, feedback_agent, meta_agent],
    tasks=[generate_recommendation, simulate_feedback, adjust_weights],
    verbose=False
)

# Execute the workflow
result = crew.kickoff()
print("\nFinal Results:", result)
```


<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #800080; text-decoration-color: #800080">╭─────────────────────────────────────────────── 🤖 Agent Started ────────────────────────────────────────────────╮</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Agent: </span><span style="color: #00ff00; text-decoration-color: #00ff00; font-weight: bold">Travel destination recommender</span>                                                                          <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Task: </span><span style="color: #00ff00; text-decoration-color: #00ff00">Use the recommend_destination tool with these preferences: {'budget': 0.33, 'luxury': 0.15, </span>             <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">'adventure': 0.52}</span>                                                                                             <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">Return only the destination name as a simple string (Paris, Bangkok, or New York).</span>                             <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace">
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #800080; text-decoration-color: #800080">╭──────────────────────────────────────────── 🔧 Agent Tool Execution ────────────────────────────────────────────╮</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Agent: </span><span style="color: #00ff00; text-decoration-color: #00ff00; font-weight: bold">Travel destination recommender</span>                                                                          <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Thought: </span><span style="color: #00ff00; text-decoration-color: #00ff00">you should always think about what to do</span>                                                              <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Using Tool: </span><span style="color: #00ff00; text-decoration-color: #00ff00; font-weight: bold">Recommend travel destination based on preferences.</span>                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #000080; text-decoration-color: #000080">╭────────────────────────────────────────────────── Tool Input ───────────────────────────────────────────────────╮</span>
<span style="color: #000080; text-decoration-color: #000080">│</span>                                                                                                                 <span style="color: #000080; text-decoration-color: #000080">│</span>
<span style="color: #000080; text-decoration-color: #000080">│</span>  <span style="color: #e6db74; text-decoration-color: #e6db74; background-color: #ffffff">"{\"user_preferences\": {\"budget\": 0.33, \"luxury\": 0.15, \"adventure\": 0.52}}"</span>                            <span style="color: #000080; text-decoration-color: #000080">│</span>
<span style="color: #000080; text-decoration-color: #000080">│</span>                                                                                                                 <span style="color: #000080; text-decoration-color: #000080">│</span>
<span style="color: #000080; text-decoration-color: #000080">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #008000; text-decoration-color: #008000">╭────────────────────────────────────────────────── Tool Output ──────────────────────────────────────────────────╮</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">New York</span>                                                                                                       <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace">
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #008000; text-decoration-color: #008000">╭───────────────────────────────────────────── ✅ Agent Final Answer ─────────────────────────────────────────────╮</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Agent: </span><span style="color: #00ff00; text-decoration-color: #00ff00; font-weight: bold">Travel destination recommender</span>                                                                          <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Final Answer:</span>                                                                                                  <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">New York</span>                                                                                                       <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace">
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #800080; text-decoration-color: #800080">╭─────────────────────────────────────────────── 🤖 Agent Started ────────────────────────────────────────────────╮</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Agent: </span><span style="color: #00ff00; text-decoration-color: #00ff00; font-weight: bold">Simulated feedback provider</span>                                                                             <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Task: </span><span style="color: #00ff00; text-decoration-color: #00ff00">Use the feedback_emulator tool with the destination from the previous task.</span>                              <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">Instructions:</span>                                                                                                  <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">1. Get the destination string from the previous task</span>                                                           <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">2. Pass it directly to the feedback_emulator tool</span>                                                              <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">3. Return the feedback value (1 or -1)</span>                                                                         <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">IMPORTANT: Pass the destination as a plain string, not a dictionary.</span>                                           <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace">
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #800080; text-decoration-color: #800080">╭──────────────────────────────────────────── 🔧 Agent Tool Execution ────────────────────────────────────────────╮</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Agent: </span><span style="color: #00ff00; text-decoration-color: #00ff00; font-weight: bold">Simulated feedback provider</span>                                                                             <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Thought: </span><span style="color: #00ff00; text-decoration-color: #00ff00">Thought: I should use the feedback emulator tool to get feedback for the destination New York.</span>        <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Using Tool: </span><span style="color: #00ff00; text-decoration-color: #00ff00; font-weight: bold">User feedback emulator tool</span>                                                                        <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #000080; text-decoration-color: #000080">╭────────────────────────────────────────────────── Tool Input ───────────────────────────────────────────────────╮</span>
<span style="color: #000080; text-decoration-color: #000080">│</span>                                                                                                                 <span style="color: #000080; text-decoration-color: #000080">│</span>
<span style="color: #000080; text-decoration-color: #000080">│</span>  <span style="color: #e6db74; text-decoration-color: #e6db74; background-color: #ffffff">"{\"destination\": \"New York\"}"</span>                                                                              <span style="color: #000080; text-decoration-color: #000080">│</span>
<span style="color: #000080; text-decoration-color: #000080">│</span>                                                                                                                 <span style="color: #000080; text-decoration-color: #000080">│</span>
<span style="color: #000080; text-decoration-color: #000080">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #008000; text-decoration-color: #008000">╭────────────────────────────────────────────────── Tool Output ──────────────────────────────────────────────────╮</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">-1</span>                                                                                                             <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace">
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #008000; text-decoration-color: #008000">╭───────────────────────────────────────────── ✅ Agent Final Answer ─────────────────────────────────────────────╮</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Agent: </span><span style="color: #00ff00; text-decoration-color: #00ff00; font-weight: bold">Simulated feedback provider</span>                                                                             <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Final Answer:</span>                                                                                                  <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">-1</span>                                                                                                             <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace">
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #800080; text-decoration-color: #800080">╭─────────────────────────────────────────────── 🤖 Agent Started ────────────────────────────────────────────────╮</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Agent: </span><span style="color: #00ff00; text-decoration-color: #00ff00; font-weight: bold">Preference weight adjuster</span>                                                                              <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Task: </span><span style="color: #00ff00; text-decoration-color: #00ff00">Use the update_weights_on_feedback tool with:</span>                                                            <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">1. destination: Get from first task's output (context[0])</span>                                                      <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">2. feedback: Get from second task's output (context[1])</span>                                                        <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">3. adjustment_factor: a number between 0 and 1 that will be used to adjust internal weights based on feedback</span>  <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">Ensure all inputs are in their correct types (string for destination, integer for feedback).</span>                   <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace">
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #800080; text-decoration-color: #800080">╭──────────────────────────────────────────── 🔧 Agent Tool Execution ────────────────────────────────────────────╮</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Agent: </span><span style="color: #00ff00; text-decoration-color: #00ff00; font-weight: bold">Preference weight adjuster</span>                                                                              <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Thought: </span><span style="color: #00ff00; text-decoration-color: #00ff00">Thought: I need to adjust the preference weights based on the provided feedback regarding New York, </span>  <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">which is a dissatisfaction (feedback score of -1).</span>                                                             <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Using Tool: </span><span style="color: #00ff00; text-decoration-color: #00ff00; font-weight: bold">Reasoning tool to adjust preference weights based on user feedback.</span>                                <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #000080; text-decoration-color: #000080">╭────────────────────────────────────────────────── Tool Input ───────────────────────────────────────────────────╮</span>
<span style="color: #000080; text-decoration-color: #000080">│</span>                                                                                                                 <span style="color: #000080; text-decoration-color: #000080">│</span>
<span style="color: #000080; text-decoration-color: #000080">│</span>  <span style="color: #e6db74; text-decoration-color: #e6db74; background-color: #ffffff">"{\"destination\": \"New York\", \"feedback\": -1, \"adjustment_factor\": 0.5}"</span>                                <span style="color: #000080; text-decoration-color: #000080">│</span>
<span style="color: #000080; text-decoration-color: #000080">│</span>                                                                                                                 <span style="color: #000080; text-decoration-color: #000080">│</span>
<span style="color: #000080; text-decoration-color: #000080">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #008000; text-decoration-color: #008000">╭────────────────────────────────────────────────── Tool Output ──────────────────────────────────────────────────╮</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">{'budget': 0.45, 'luxury': 0.22, 'adventure': 0.33}</span>                                                            <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace">
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #008000; text-decoration-color: #008000">╭───────────────────────────────────────────── ✅ Agent Final Answer ─────────────────────────────────────────────╮</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Agent: </span><span style="color: #00ff00; text-decoration-color: #00ff00; font-weight: bold">Preference weight adjuster</span>                                                                              <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Final Answer:</span>                                                                                                  <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">{'budget': 0.45, 'luxury': 0.22, 'adventure': 0.33}</span>                                                            <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace">
</pre>



    
    Final Results: {'budget': 0.45, 'luxury': 0.22, 'adventure': 0.33}


Figure 4.1 shows a visual understanding of this flow:

![Meta-reasoning with AI agents and CrewAI framework](figures/Figure_4.1–Meta-reasoning_with_AI_agents_and_CrewAI_framework.png)


# 2. Self-Explanation - example

### **What is Self-explanation?**

**Self-explanation** is a process through which agents verbalize their reasoning processes, generating
explanations for decisions reached. This technique serves several crucial purposes for reflective agents,
particularly in the context of our travel agent example, as discussed in the following sections.

Self-explanation serves two distinct purposes: _**enhancing transparency**_ and _**facilitating learning**_.


In this section, we will see how to implement transparency, learning and refinement, user engagement & collaboration.

1. **Self-Explanation transparency**: For each recommendation, the agent generates a detailed self-explanation. This explanation outlines the factors that led to the recommendation, such as proximity to popular attractions, budget-friendly options, or the presence of adventure activities. The purpose is to provide transparency into how the decision was made, helping the user understand the reasoning process.

2. **Learning and refinement**: The agent doesn't stop after making the recommendation. It actively reflects on user feedback (whether positive or negative). If the feedback is negative, it introspects on its decision-making process and adjusts the importance (weights) it assigns to user preferences for future recommendations. For instance, if a user dislikes a budget-friendly recommendation, the agent might reduce the emphasis it places on budget-related preferences.

3. **User Engagement**: The class also simulates a dialogue with the user. After giving the recommendation and the self-explanation, it collects feedback from the user, allowing for a more collaborative interaction. This feedback is then used to refine future recommendations, making the agent more adaptive and personalized.




```python
import getpass
import os

api_key = getpass.getpass(prompt="Enter OpenAI API Key: ")
os.environ["OPENAI_API_KEY"] = api_key
```

    Enter OpenAI API Key:  ········


### 2.1 Transparency: Verbalizing Reasoning in Decisions

Lets use OpenAI SDK to see how a model can perform reasoning in the decisions it makes. Here, the agent generates explanations for its reasoning when recommending a travel itinerary. It uses GPT-4o-mini to generate self-explanations.


```python
import openai

# Mock data for the travel recommendation
user_preferences = {
    "location": "Paris",
    "budget": 200,
    "preferences": ["proximity to attractions", "user ratings"],
}

# Input reasoning factors for the GPT model
reasoning_prompt = f"""
You are an AI-powered travel assistant. Explain your reasoning behind a hotel recommendation for a user traveling to {user_preferences['location']}.
Consider:
1. Proximity to popular attractions.
2. High ratings from similar travelers.
3. Competitive pricing within ${user_preferences['budget']} budget.
4. Preferences: {user_preferences['preferences']}.
Provide a clear, transparent self-explanation.
"""

response = openai.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {"role": "system", "content": "You are a reflective travel assistant."},
        {"role": "user", "content": reasoning_prompt},
    ]
)

# Print self-explanation
print("Agent Self-Explanation:")
print(response.choices[0].message.content)
```

    Agent Self-Explanation:
    When recommending a hotel for a user traveling to Paris, I will take into account the preferences specified, namely proximity to attractions and high user ratings, while also adhering to the budget of $200.
    
    1. **Proximity to Popular Attractions**: Paris is filled with iconic sites such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and Montmartre. A good choice would be a hotel located in areas such as the 1st (near the Louvre), the 7th (near the Eiffel Tower), or the 18th (Montmartre). This ensures that the user can easily access these attractions, potentially minimizing travel time and maximizing the overall experience.
    
    2. **High Ratings from Similar Travelers**: I would search for hotels that have received excellent ratings on travel platforms, typically around 4.0 out of 5.0 or better. This provides assurance that previous guests had positive experiences, contributing to the reliability of the recommendation. Feedback regarding cleanliness, service, and amenities would play a crucial role to ensure that the user has a comfortable stay.
    
    3. **Competitive Pricing within $200 Budget**: It’s important to filter hotels that fall within the user's financial constraints. The cost consideration ensures that the user can enjoy their trip without overextending their budget. I would look for hotels that not only meet the accommodation needs but also offer good value for the price.
    
    Given these considerations, a recommended hotel might be something like **Hôtel le Relais Saint-Honoré** in the 1st arrondissement. This hotel typically offers competitive pricing around the $200 mark, boasts high ratings from guests, and is just a short walk from several major attractions such as the Louvre and the Palais Royal. 
    
    To summarize, my recommendation for a hotel in Paris hinges on ensuring that it is well-located for easy access to key sites, maintains high user ratings reflecting positive traveler experiences, and stays within an affordable price range that aligns with the user's budget. This holistic approach helps ensure a fulfilling trip to Paris, emphasizing both convenience and guest satisfaction.


**Answer:**

Agent Self-Explanation:
When recommending a hotel for a user traveling to Paris, I will take into account the preferences specified, namely proximity to attractions and high user ratings, while also adhering to the budget of $200.

1. **Proximity to Popular Attractions**: Paris is filled with iconic sites such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and Montmartre. A good choice would be a hotel located in areas such as the 1st (near the Louvre), the 7th (near the Eiffel Tower), or the 18th (Montmartre). This ensures that the user can easily access these attractions, potentially minimizing travel time and maximizing the overall experience.

2. **High Ratings from Similar Travelers**: I would search for hotels that have received excellent ratings on travel platforms, typically around 4.0 out of 5.0 or better. This provides assurance that previous guests had positive experiences, contributing to the reliability of the recommendation. Feedback regarding cleanliness, service, and amenities would play a crucial role in ensuring that the user has a comfortable stay.

3. **Competitive Pricing within $200 Budget**: It’s important to filter hotels that fall within the user's financial constraints. The cost consideration ensures that the user can enjoy their trip without overextending their budget. I would look for hotels that not only meet the accommodation needs but also offer good value for the price.

Given these considerations, a recommended hotel might be something like **Hôtel le Relais Saint-Honoré** in the 1st arrondissement. This hotel typically offers competitive pricing around the $200 mark, boasts high ratings from guests, and is just a short walk from several major attractions such as the Louvre and the Palais Royal. 

To summarize, my recommendation for a hotel in Paris hinges on ensuring that it is well-located for easy access to key sites, maintains high user ratings reflecting positive traveler experiences, and stays within an affordable price range that aligns with the user's budget. This holistic approach helps ensure a fulfilling trip to Paris, emphasizing both convenience and guest satisfaction.
  

A conceptual flow of how an agentic system with self-explanation and transparency would look is
shown in Figure 4.2:

![Self-explanations transparency with AI agents](figures/Figure_4.2–Self-explanations_transparency_with_AI_agents.png)

#### Using Crew AI

Our previous example was pretty simple and didn't use Agents. But with an agentic system you may have agents actually lookup hotels appropriate to the user query using tools. Subsequently, a second agent may perform the self-explanation transparency on the response. Let's first define a tool that will respond back with mock hotel data based on price.


```python
from crewai.tools import tool

# Tool 1
@tool("Recommend hotels based on user query.")
def recommend_hotel(cost_per_night: int) -> str:
    """
    Returns hotels based on cost per night.

    Args:
        cost_per_night (int): User's preference for hotel room per night cost. 
            
    """
    static_hotels = [
        {
            "hotel_name": "Le Royal Monceau Raffles",
            "price_per_night": 1200,
            "transportation_convenience": "convenient",
            "location": "8th arrondissement",
            "nearest_metro": "Charles de Gaulle-Étoile",
            "distance_from_metro": '1 km'
        },
        {
            "hotel_name": "Citadines Les Halles",
            "price_per_night": 250,
            "transportation_convenience": "convenient",
            "location": "1st arrondissement",
            "nearest_metro": "Les Halles",
            "distance_from_metro": '2.8 km'
        },
        {
            "hotel_name": "Ibis Paris Montmartre",
            "price_per_night": 120,
            "transportation_convenience": "moderate",
            "location": "18th arrondissement",
            "nearest_metro": "Place de Clichy",
            "distance_from_metro": '5 km'
        },
        {
            "hotel_name": "Four Seasons Hotel George V",
            "price_per_night": 1500,
            "transportation_convenience": "convenient",
            "location": "8th arrondissement",
            "nearest_metro": "George V",
            "distance_from_metro": '1 km'
        },
        {
            "hotel_name": "Hotel du Petit Moulin",
            "price_per_night": 300,
            "transportation_convenience": "moderate",
            "location": "3rd arrondissement",
            "nearest_metro": "Saint-Sébastien Froissart",
            "distance_from_metro": '1.9 km'
        }
    ]
    matching_hotels = [
        hotel for hotel in static_hotels 
        if cost_per_night <= hotel["price_per_night"]
    ]
    return matching_hotels
```

Now we will perform the same transparency reasoning with a CrewAI Agent/Task combination.

### Transparency

_By generating self-explanations for its recommendations and decisions, the reflective travel agent can
provide users with insights into its thought processes and decision-making rationale. This transparency
fosters trust and confidence in the agent’s capabilities, as users can better understand the reasoning
behind the suggested itineraries, accommodations, or activities_.


```python
from crewai import Agent, Task, Crew
from crewai.process import Process

travel_agent = Agent(
    role="Travel Advisor",
    goal="Provide hotel recommendations with transparent reasoning.",
    backstory="""
    An AI travel advisor specializing in personalized travel planning. 
    You always explain the steps you take to arrive at a conclusion
    """,
    allow_delegation=False,
    llm='gpt-4o-mini',
    tools=[recommend_hotel]
)

recommendation_task = Task(
    name="Recommend hotel",
    description="""
    Recommend a hotel based on the user's query: 
    '{query}'.
    """,
    agent=travel_agent,
    expected_output="The hotel recommendation and reasoning in the following format\n\nHotel: [Your answer]\n\nPrice/night: [The price]\n\nReason: [Detailed breakdown of your thought process]"
)

travel_crew = Crew(
    agents=[travel_agent],
    tasks=[recommendation_task],
    process=Process.sequential,
    verbose=False
)

travel_crew.kickoff(inputs={'query': "I am looking for a hotel in Paris under $300 a night."})

# Retrieve and print the output
output = recommendation_task.output
print("Hotel Recommendation and Explanation:")
print(output)
```

    Hotel Recommendation and Explanation:
    Hotel: Hotel du Petit Moulin
    
    Price/night: $300
    
    Reason: After analyzing various hotel options in Paris under the budget of $300 per night, I found that the Hotel du Petit Moulin is the only viable option. This hotel is located in the 3rd arrondissement, which is known for its vibrant atmosphere. It has a moderate level of transportation convenience, with the nearest metro station being Saint-Sébastien Froissart, roughly 1.9 km away. Given its unique decor and central location, it offers a good balance of comfort and accessibility within the user's budget.


Not only does our Agent can lookup hotels using the tool but it clearly explains why it gave the recommendation as `Reason`.

### 2.2 Learning and Refinement: Using Self-Explanation to Identify Gaps

While our self-explaining agent is great, the user may still not like the recommendation it gave. In which case, the user may express their dissatisfaction with the recommendation. This is where we need learning and refinement of the approach. In our case, we may extend the previous agent-based system to now also include a second agent that can take user feedback and re-strategize on its approach to recommend a hotel. Note that in this case, sequential execution or parallel execution of the agents may not be appropriate; thus, we need a hierarchical approach where a top-level agent can manage the two agents and then delegate tasks accordingly.

Let's define our **learning** and **refinement** agent that can take user feedback and its previous recommendation in context, and then complete the task by refining its strategy.


```python
from crewai import Agent, Task, Crew
from crewai.process import Process

# Agent Definition
reflective_travel_agent = Agent(
    role="Self-Improving Travel Advisor",
    goal="Refine hotel recommendations based on user feedback to your previous recommendation to improve decision-making.",
    backstory="""
    A reflective AI travel advisor specializing in personalized travel planning that learns from user feedback. 
    When a user highlights an issue with a recommendation, it revisits its reasoning,
    identifies overlooked factors, and updates its decision process accordingly.
    """,
    allow_delegation=False,
    llm='gpt-4o-mini',
    #tools=[recommend_hotel] # <-- This agent also uses the same tool to refine its recommendation
)

# Task Definition
feedback_task = Task(
    description="""
    Based on your previous recommendation:
    '{recommendation}'

    Reflect on the user's feedback on the hotel recommendation:
    '{query}'

    - Identify any oversight in your previous reasoning process.
    - Update your reasoning process to include aspects that were missed.
    - Provide the refined steps that you will use to recommend hotels.
    """,
    expected_output="""
    A refined explanation that acknowledges the oversight, includes missed factors,
    and provides revised steps to recommend hotels tailored to the user's feedback.
    """,
    agent=reflective_travel_agent,
    context=[recommendation_task] 
)

# Crew Orchastartion
travel_feedback_crew = Crew(
    agents=[reflective_travel_agent],
    tasks=[feedback_task],
    process=Process.sequential,
    verbose=False
)

# We will run the travel_crew from the previous example with user's query
response1 = travel_crew.kickoff(inputs={'query': "I am looking for a hotel in Paris under $300 a night."})
print(response1)

# Adjusted code
response2 = travel_feedback_crew.kickoff(inputs={
    'recommendation': str(response1),  # Convert CrewOutput to string
    'query': "The hotel you recommended was too far from public transport. I prefer locations closer to metro stations."
})

print(response2)

```

    Hotel: Hotel du Petit Moulin
    
    Price/night: $300
    
    Reason: "Hotel du Petit Moulin is the only recommendation that fits the user's budget of under $300 per night. It is located in the 3rd arrondissement, which is a vibrant area known for its cafes and boutiques. The nearest metro station is Saint-Sébastien Froissart, approximately 1.9 km away, providing moderate transportation convenience. This hotel offers a unique charm, making it a suitable choice for a pleasant stay in Paris."
    I appreciate your feedback regarding the hotel's proximity to public transport, as this is crucial for ensuring an enjoyable experience during your travels. Upon reflection, I realize that my previous recommendation of Hotel du Petit Moulin did not adequately consider the desire for closer access to metro stations, particularly as the distance of approximately 1.9 km could pose an inconvenience.
    
    In addition to distance, I should also take into account the types of metro stations available nearby, as some lines might provide more convenient access to major attractions in Paris. Furthermore, a review of nearby amenities or services, like restaurants or grocery stores, could enhance the overall experience of your stay.
    
    To better refine my process for future hotel recommendations, I will adopt the following updated reasoning steps:
    
    1. **User Preferences Assessment**: Begin by confirming the user's specific preferences regarding proximity to transport. 
    2. **Transport Accessibility Evaluation**: Identify hotels within a defined walking distance (e.g., less than 0.5 km) from metro stations, ensuring excellent access to key transport routes.
    3. **Budget Consideration**: Verify that hotel options still fit within the user’s budget constraints.
    4. **Local Amenities**: Assess the vibrancy of the surrounding area, considering cafes, restaurants, and shops, to enhance the overall travel experience.
    5. **User Feedback Integration**: Actively seek feedback on my recommendations and adjust my approach accordingly to refine future suggestions continually. 
    
    By incorporating these steps, I can ensure that my hotel recommendations provide not only good pricing and local charm but also essential access to public transportation in accordance with your preferences. Thank you for your invaluable input, and I look forward to assisting you with future travel plans!


Figure 4.3 shows the high-level flow:

![Learning and refinement with AI agents](figures/Figure_4.3–Learning_and_refinement_with_AI_agents.png)

### 2.3. User Engagement and Collaboration: Enabling Interactive Explanations

In this example, the agent provides explanations for its decisions and engages users to refine suggestions interactively. Just like before, we can have an Agent/Task pair with CrewAI framework whose job is to interact with the users by asking clarifying questions about their preferences.


```python
from crewai import Agent, Task, Crew
from crewai.process import Process

# Step 1: Define the Collaborative Agent
collaborative_travel_agent = Agent(
    role="Collaborative AI Travel Assistant",
    goal="""
    Engage in an interactive dialogue with the user to clarify hotel recommendations.
    Explain the reasoning for prioritizing certain factors and invite the user to share their preferences.
    """,
    backstory="""
    An AI travel assistant that values user input and ensures recommendations are well-aligned with user needs.
    It provides clear explanations for its decisions and encourages collaborative planning.
    """
)

# Assemble and define the Task
interactive_task = Task(
    description="""
    Facilitate an interactive dialogue with the user.

    - Here is the initial recommendation: {initial_recommendation}
    - The user has asked: {user_query}

    Respond by:
    1. Explaining the reasoning behind prioritizing proximity to attractions.
    2. Inviting the user to clarify whether proximity to public transport is more important.
    """,
    expected_output="""
    A clear and polite response explaining the reasoning and inviting the user to share further input.
    """,
    agent=collaborative_travel_agent
)

# Step 3: Assemble the Crew
interactive_crew = Crew(
    agents=[collaborative_travel_agent],
    tasks=[interactive_task],
    process=Process.sequential,
    verbose=True
)


# Step 2: Define the Task for Clarification Dialogue

# Initial recommendation 
initial_recommendation = "I recommend Hotel Lumière in Paris for its proximity to the Eiffel Tower, high ratings, and budget-friendly price."
user_query = "Why did you prioritize proximity to attractions over public transport access?"

# Step 4: Run the Crew and Output the Results
print("Starting Interactive Dialogue with User...\n")
result = interactive_crew.kickoff(inputs={"initial_recommendation": initial_recommendation, "user_query": user_query})

print("Final Interactive Response:")
print(result)

```

    Starting Interactive Dialogue with User...
    



<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #008080; text-decoration-color: #008080">╭──────────────────────────────────────────── Crew Execution Started ─────────────────────────────────────────────╮</span>
<span style="color: #008080; text-decoration-color: #008080">│</span>                                                                                                                 <span style="color: #008080; text-decoration-color: #008080">│</span>
<span style="color: #008080; text-decoration-color: #008080">│</span>  <span style="color: #008080; text-decoration-color: #008080; font-weight: bold">Crew Execution Started</span>                                                                                         <span style="color: #008080; text-decoration-color: #008080">│</span>
<span style="color: #008080; text-decoration-color: #008080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Name: </span><span style="color: #008080; text-decoration-color: #008080">crew</span>                                                                                                     <span style="color: #008080; text-decoration-color: #008080">│</span>
<span style="color: #008080; text-decoration-color: #008080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">ID: </span><span style="color: #008080; text-decoration-color: #008080">3e5d29cb-e73c-4b58-a3ea-ba90c73ace3c</span>                                                                       <span style="color: #008080; text-decoration-color: #008080">│</span>
<span style="color: #008080; text-decoration-color: #008080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Tool Args: </span>                                                                                                    <span style="color: #008080; text-decoration-color: #008080">│</span>
<span style="color: #008080; text-decoration-color: #008080">│</span>                                                                                                                 <span style="color: #008080; text-decoration-color: #008080">│</span>
<span style="color: #008080; text-decoration-color: #008080">│</span>                                                                                                                 <span style="color: #008080; text-decoration-color: #008080">│</span>
<span style="color: #008080; text-decoration-color: #008080">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace">
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"></pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #800080; text-decoration-color: #800080">╭─────────────────────────────────────────────── 🤖 Agent Started ────────────────────────────────────────────────╮</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Agent: </span><span style="color: #00ff00; text-decoration-color: #00ff00; font-weight: bold">Collaborative AI Travel Assistant</span>                                                                       <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Task: </span>                                                                                                         <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">    Facilitate an interactive dialogue with the user.</span>                                                          <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">    - Here is the initial recommendation: I recommend Hotel Lumière in Paris for its proximity to the Eiffel </span>  <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">Tower, high ratings, and budget-friendly price.</span>                                                                <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">    - The user has asked: Why did you prioritize proximity to attractions over public transport access?</span>        <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">    Respond by:</span>                                                                                                <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">    1. Explaining the reasoning behind prioritizing proximity to attractions.</span>                                  <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">    2. Inviting the user to clarify whether proximity to public transport is more important.</span>                   <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">    </span>                                                                                                           <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">│</span>                                                                                                                 <span style="color: #800080; text-decoration-color: #800080">│</span>
<span style="color: #800080; text-decoration-color: #800080">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace">
</pre>




    Output()



<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"></pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #008000; text-decoration-color: #008000">╭───────────────────────────────────────────── ✅ Agent Final Answer ─────────────────────────────────────────────╮</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Agent: </span><span style="color: #00ff00; text-decoration-color: #00ff00; font-weight: bold">Collaborative AI Travel Assistant</span>                                                                       <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Final Answer:</span>                                                                                                  <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">Thank you for your question! I prioritized proximity to attractions like the Eiffel Tower because it enhances</span>  <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">convenience for sightseeing, allowing you to maximize your time exploring the iconic landmarks of Paris. </span>      <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">Being within walking distance can also lead to a more enjoyable and immersive experience without the need for</span>  <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">extensive travel time. </span>                                                                                        <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">However, I completely understand that some travelers place a high value on access to public transport, as it </span>  <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">can provide flexibility in exploring other areas of the city and may be beneficial for longer trips outside </span>   <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">the immediate vicinity. </span>                                                                                       <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">Could you share if proximity to public transport is a higher priority for your travel needs, or are there </span>     <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #00ff00; text-decoration-color: #00ff00">specific attractions you’re particularly keen on visiting? Your input will help me refine my recommendations!</span>  <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace">
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"></pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #008000; text-decoration-color: #008000">╭──────────────────────────────────────────────── Task Completion ────────────────────────────────────────────────╮</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #008000; text-decoration-color: #008000; font-weight: bold">Task Completed</span>                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Name: </span><span style="color: #008000; text-decoration-color: #008000">dd082fa4-9690-48cb-a186-3b8628e5ac57</span>                                                                     <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Agent: </span><span style="color: #008000; text-decoration-color: #008000">Collaborative AI Travel Assistant</span>                                                                       <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Tool Args: </span>                                                                                                    <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace">
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace"><span style="color: #008000; text-decoration-color: #008000">╭──────────────────────────────────────────────── Crew Completion ────────────────────────────────────────────────╮</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #008000; text-decoration-color: #008000; font-weight: bold">Crew Execution Completed</span>                                                                                       <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Name: </span><span style="color: #008000; text-decoration-color: #008000">crew</span>                                                                                                     <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">ID: </span><span style="color: #008000; text-decoration-color: #008000">3e5d29cb-e73c-4b58-a3ea-ba90c73ace3c</span>                                                                       <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Tool Args: </span>                                                                                                    <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Final Output: Thank you for your question! I prioritized proximity to attractions like the Eiffel Tower </span>       <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">because it enhances convenience for sightseeing, allowing you to maximize your time exploring the iconic </span>      <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">landmarks of Paris. Being within walking distance can also lead to a more enjoyable and immersive experience </span>  <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">without the need for extensive travel time. </span>                                                                   <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">However, I completely understand that some travelers place a high value on access to public transport, as it </span>  <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">can provide flexibility in exploring other areas of the city and may be beneficial for longer trips outside </span>   <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">the immediate vicinity. </span>                                                                                       <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">Could you share if proximity to public transport is a higher priority for your travel needs, or are there </span>     <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>  <span style="color: #c0c0c0; text-decoration-color: #c0c0c0">specific attractions you’re particularly keen on visiting? Your input will help me refine my recommendations!</span>  <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">│</span>                                                                                                                 <span style="color: #008000; text-decoration-color: #008000">│</span>
<span style="color: #008000; text-decoration-color: #008000">╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯</span>
</pre>




<pre style="white-space:pre;overflow-x:auto;line-height:normal;font-family:Menlo,'DejaVu Sans Mono',consolas,'Courier New',monospace">
</pre>



    Final Interactive Response:
    Thank you for your question! I prioritized proximity to attractions like the Eiffel Tower because it enhances convenience for sightseeing, allowing you to maximize your time exploring the iconic landmarks of Paris. Being within walking distance can also lead to a more enjoyable and immersive experience without the need for extensive travel time. 
    
    However, I completely understand that some travelers place a high value on access to public transport, as it can provide flexibility in exploring other areas of the city and may be beneficial for longer trips outside the immediate vicinity. 
    
    Could you share if proximity to public transport is a higher priority for your travel needs, or are there specific attractions you’re particularly keen on visiting? Your input will help me refine my recommendations!


# 3. Self Modeling - example

**Self-modeling** is a crucial aspect of reflective agents, allowing them to maintain an internal representation
of their goals, beliefs, and knowledge. This self-model serves as a foundation for decision-making and
reflection, enabling the agent to adapt and evolve in response to changing circumstances or newly
acquired information. To clarify a bit further, the term modeling in this context means the agent’s initial
environment and state. The agent (or group of agents) starts with some initial state with a specific
environment, and as the agent learns more via human-machine interactions or via its task executions,
it continues to update that internal state, thus changing its own environment within which it operates.

The `ReflectiveTravelAgentWithSelfModeling` class represents a sophisticated travel recommendation system that utilizes **self-modeling** to enhance its decision-making and adaptability. 

### 1. **Initialization:**
   - **Self-Model and Knowledge Base:** The agent starts with an internal model that includes its goals and a knowledge base. 
     - **Goals:** Initially, the goals are set to provide personalized recommendations, optimize user satisfaction, and not prioritize eco-friendly options by default.
     - **Knowledge Base:** It contains information about various travel destinations, including their ratings, costs, luxury levels, and sustainability. This base also tracks user preferences.

### 2. **Updating Goals:**
   - **Adapting to Preferences:** When new user preferences are provided, the agent can update its goals accordingly. For example, if the user prefers eco-friendly options, the agent will adjust its goals to prioritize recommending sustainable travel options. Similarly, if the user’s budget changes, the agent will refocus on cost-effective recommendations.

### 3. **Updating Knowledge Base:**
   - **Incorporating Feedback:** After receiving feedback from users, the agent updates its knowledge base. If the feedback is positive, the agent increases the rating of the recommended destination. If the feedback is negative, the rating is decreased. This helps the agent refine its recommendations based on real user experiences.

### 4. **Making Recommendations:**
   - **Calculating Scores:** The agent evaluates each destination by calculating a score based on its rating and, if eco-friendly options are a goal, it adjusts the score by adding the sustainability rating.
   - **Selecting the Best Destination:** The destination with the highest score is recommended to the user. This process ensures that the recommendation aligns with both user preferences and the agent’s goals.

### 5. **Engaging with the User:**
   - **Providing Recommendations:** The agent presents the recommended destination to the user and asks for feedback.
   - **Feedback Handling:** The feedback (positive or negative) is used to update the knowledge base, which helps improve future recommendations.

_Figure 4.4_ gives a high-level overview of agent self-modeling as we further discuss the two components of internal state. Agents may have individual internal states that they independently self-model within an agentic system, or they may have a shared internal state that they collaboratively self-model.

![Individual and shared internal states for self-modeling](figures/Figure_4.4–Individual_and_shared_internal_states_for_self-modeling.png)


```python
class ReflectiveTravelAgentWithSelfModeling:
    def __init__(self):
        # Initialize the agent with a self-model that includes goals and a knowledge base
        self.self_model = {
            "goals": {
                "personalized_recommendations": True,
                "optimize_user_satisfaction": True,
                "eco_friendly_options": False  # Default: Not prioritizing eco-friendly options
            },
            "knowledge_base": {
                "destinations": {
                    "Paris": {"rating": 4.8, "cost": 2000, "luxury": 0.9, "sustainability": 0.3},
                    "Bangkok": {"rating": 4.5, "cost": 1500, "luxury": 0.7, "sustainability": 0.6},
                    "Barcelona": {"rating": 4.7, "cost": 1800, "luxury": 0.8, "sustainability": 0.7}
                },
                "user_preferences": {}
            }
        }

    def update_goals(self, new_preferences):
        """Update the agent's goals based on new user preferences."""
        if new_preferences.get("eco_friendly"):
            self.self_model["goals"]["eco_friendly_options"] = True
            print("Updated goal: Prioritize eco-friendly travel options.")
        if new_preferences.get("adjust_budget"):
            print("Updated goal: Adjust travel options based on new budget constraints.")
    
    def update_knowledge_base(self, feedback):
        """Update the agent's knowledge base based on user feedback."""
        destination = feedback["destination"]
        if feedback["positive"]:
            # Increase rating for positive feedback
            self.self_model["knowledge_base"]["destinations"][destination]["rating"] += 0.1
            print(f"Positive feedback received for {destination}; rating increased.")
        else:
            # Decrease rating for negative feedback
            self.self_model["knowledge_base"]["destinations"][destination]["rating"] -= 0.2
            print(f"Negative feedback received for {destination}; rating decreased.")
    
    def recommend_destination(self, user_preferences):
        """Recommend a destination based on user preferences and the agent's self-model."""
        # Store user preferences in the agent's self-model
        self.self_model["knowledge_base"]["user_preferences"] = user_preferences
        
        # Update agent's goals based on new preferences
        if user_preferences.get("eco_friendly"):
            self.update_goals(user_preferences)
        
        # Calculate scores for each destination
        best_destination = None
        highest_score = 0
        for destination, info in self.self_model["knowledge_base"]["destinations"].items():
            score = info["rating"]
            if self.self_model["goals"]["eco_friendly_options"]:
                # Boost score for eco-friendly options if that goal is prioritized
                score += info["sustainability"]
            
            # Update the best destination if the current score is higher
            if score > highest_score:
                best_destination = destination
                highest_score = score
        
        return best_destination

    def engage_with_user(self, destination):
        """Simulate user engagement by providing the recommendation and receiving feedback."""
        print(f"Recommended destination: {destination}")
        # Simulate receiving user feedback (e.g., through input in a real application)
        feedback = input(f"Did you like the recommendation of {destination}? (yes/no): ").strip().lower()
        positive_feedback = feedback == "yes"
        return {"destination": destination, "positive": positive_feedback}



```

The provided code snippet is designed to simulate the usage of the `ReflectiveTravelAgentWithSelfModeling` class. 

### 1. **Creating an Instance of the Agent:**
   ```python
   agent = ReflectiveTravelAgentWithSelfModeling()
   ```
   - **Purpose:** Initializes a new instance of the `ReflectiveTravelAgentWithSelfModeling` class.
   - **Outcome:** This instance represents a travel agent equipped with self-modeling capabilities, including goal management and a knowledge base.

### 2. **Setting User Preferences:**
   ```python
   user_preferences = {
       "budget": 0.6,            # Moderate budget constraint
       "luxury": 0.4,            # Moderate preference for luxury
       "adventure": 0.7,         # High preference for adventure
       "eco_friendly": True      # User prefers eco-friendly options
   }
   ```
   - **Purpose:** Defines a set of preferences provided by the user.
   - **Outcome:** These preferences indicate that the user has a moderate budget, moderate luxury preferences, a high interest in adventure, and a strong preference for eco-friendly options.

### 3. **Getting a Recommendation:**
   ```python
   recommendation = agent.recommend_destination(user_preferences)
   ```
   - **Purpose:** Requests a travel destination recommendation from the agent based on the provided user preferences.
   - **Outcome:** The agent processes the preferences, updates its goals if necessary (e.g., prioritizing eco-friendly options), and selects the best destination to recommend.

### 4. **Engaging with the User:**
   ```python
   feedback = agent.engage_with_user(recommendation)
   ```
   - **Purpose:** Simulates interaction with the user by presenting the recommendation and gathering feedback.
   - **Outcome:** The user provides feedback on the recommended destination, which is used to evaluate the effectiveness of the recommendation.

### 5. **Updating the Knowledge Base:**
   ```python
   agent.update_knowledge_base(feedback)
   ```
   - **Purpose:** Updates the agent’s knowledge base with the feedback received from the user.
   - **Outcome:** The agent adjusts its knowledge base by modifying ratings or other attributes based on whether the feedback was positive or negative. This update helps improve future recommendations by refining the agent's understanding of user preferences and destination qualities.

### Summary:
In essence, this code snippet demonstrates how the `ReflectiveTravelAgentWithSelfModeling` class operates in a simulated environment. It initializes the agent, sets user preferences, obtains a recommendation, engages the user for feedback, and updates the agent’s knowledge base based on that feedback. This simulation helps illustrate the agent’s self-modeling capabilities and its ability to adapt and improve recommendations over time.


```python
# Simulating agent usage
if __name__ == "__main__":
    # Create an instance of the reflective travel agent with self-modeling
    agent = ReflectiveTravelAgentWithSelfModeling()
    
    # Example user preferences including a focus on eco-friendly options
    user_preferences = {
        "budget": 0.6,            # Moderate budget constraint
        "luxury": 0.4,            # Moderate preference for luxury
        "adventure": 0.7,         # High preference for adventure
        "eco_friendly": True      # User prefers eco-friendly options
    }
    
    # Get the recommended destination based on user preferences
    recommendation = agent.recommend_destination(user_preferences)
    
    # Engage with the user to provide feedback on the recommendation
    feedback = agent.engage_with_user(recommendation)
    
    # Update the knowledge base with the user feedback
    agent.update_knowledge_base(feedback)

```

    Updated goal: Prioritize eco-friendly travel options.
    Recommended destination: Barcelona


    Did you like the recommendation of Barcelona? (yes/no):  no


    Negative feedback received for Barcelona; rating decreased.



```python

```
