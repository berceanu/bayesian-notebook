FROM jupyter/minimal-notebook

# Add RUN statements to install packages as the $NB_USER defined in the base images.

# Add a "USER root" statement followed by RUN statements to install system packages using apt-get,
# change file permissions, etc.

# If you do switch to root, always be sure to add a "USER $NB_USER" command at the end of the
# file to ensure the image runs as a unprivileged user by default.

# Fix: https://github.com/hadolint/hadolint/wiki/DL4006
# Fix: https://github.com/koalaman/shellcheck/wiki/SC3014
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root
WORKDIR /

# Add environment files
RUN mkdir /environments
COPY python3.yml /environments/
RUN rm "${CONDA_DIR}/conda-meta/pinned"

USER ${NB_UID}



RUN mamba install -n base -y python=3.10* && \
    CONDA_OVERRIDE_CUDA="11.2" mamba env update --quiet -n base -f /environments/python3.yml && \
    mamba clean --all -f -y && \
    mamba list python | grep '^python ' | tr -s ' ' | cut -d ' ' -f 1,2 >> "${CONDA_DIR}/conda-meta/pinned" && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"


USER ${NB_UID}

WORKDIR "${HOME}"

