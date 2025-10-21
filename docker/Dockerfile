# Start with BioSim base image.
ARG BASE_IMAGE=latest
FROM ghcr.io/ccpbiosim/jupyterhub-base:$BASE_IMAGE

LABEL maintainer="James Gebbie-Rayet <james.gebbie@stfc.ac.uk>"

ARG TARGETPLATFORM

# Switch to jovyan user.
USER $NB_USER
WORKDIR $HOME

# Dependencies for the workshop
RUN if [ "$TARGETPLATFORM" = "linux/amd64" ]; then \
      conda install conda-forge/linux-64::gromacs=2024.5=nompi_h5f56185_100 -y; \
    elif [ "$TARGETPLATFORM" = "linux/arm64" ]; then \
      conda install conda-forge/linux-aarch64::gromacs=2024.5=nompi_h9afd374_100 -y; \
    fi
RUN conda install ipywidgets nglview -y
RUN pip install mdtraj

# Get workshop files and move them to jovyan directory.
COPY --chown=1000:100 . .
RUN rm -rf LICENSE README.md docker .git .github

# Copy lab workspace
COPY --chown=1000:100 docker/default-37a8.jupyterlab-workspace /home/jovyan/.jupyter/lab/workspaces/default-37a8.jupyterlab-workspace

# Always finish with non-root user as a precaution.
USER $NB_USER
