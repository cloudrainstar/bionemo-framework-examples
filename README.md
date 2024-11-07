# BioNeMo Framework Examples

## Step 1 - Launch Container and Download Models

When launching docker container, in order to avoid downloading models, mount a persistent volume to /workspace/bionemo/models.

```bash
docker run -it --rm \
  --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 \
  --shm-size=1g \
  -v $PWD/models:/workspace/bionemo/models \ # This mounts the model folder so we can persist models
  -e NGC_CLI_API_KEY=<NGC_CLI_API_KEY> \ # This is required for downloading models
  -p 8888:8888 \ # Ports
  nvcr.io/nvidia/clara/bionemo-framework:1.10 \
  bash
```

After you launch the docker, you should be in a bash environment inside the container. From here, just run `./launch.sh download` to download all the BioNeMo models. After the models are downloaded, `exit` the container.

## Step 2 - Launch Container in Inference Mode

```bash
docker run -it --rm \
  --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 \
  --shm-size=1g \
  -v $PWD/data:/workspace/bionemo/examples/protein/esm1nv/nbs/data \ # This mounts the data folder into the Jupyter Environment
  -v $PWD/models:/workspace/bionemo/models \ # This mounts the model folder so we can persist models
  --gpus device=<GPU-UUID> \ # This can be gpus all if on a system with only one GPU
  -e NGC_CLI_API_KEY=<NGC_CLI_API_KEY> \ # This is required for downloading models
  -p 8888:8888 \ # Ports
  nvcr.io/nvidia/clara/bionemo-framework:1.10 \
  esm-1nv    # Change models as needed.
```

There is currently a bug and the inference service will not work shortly after startup. However, we can still use the interactive inference, so it will be fine.

Open the jupyter lab environment at `http://localhost:8888/lab` or if it's a remote machine `http://<ip>:8888/lab`.

Grab the `Testset.ipynb` from this repository and drop it into the data folder, as well as the tsv. Follow along the `Testset.ipynb` notebook to convert sequences to embedding vectors.
