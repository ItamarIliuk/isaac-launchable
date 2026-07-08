# SPDX-FileCopyrightText: Copyright (c) 2025 NVIDIA CORPORATION & AFFILIATES. All rights reserved.
# SPDX-License-Identifier: Apache-2.0
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
FROM nvcr.io/nvidia/isaac-lab:3.0.0-beta2-post1

USER root

RUN apt update && \
    apt install -y \
	build-essential \
	cmake \
	curl \
	git

RUN curl -fsSL https://code-server.dev/install.sh | sh -s -- --version=4.96.4

RUN apt update && \
	apt install -y \
	unzip \
	dnsutils \
	iproute2 \
	jq

ENV WORKSPACE_DIR=/workspace

COPY --chown=isaaclab:isaaclab ./README.md ${WORKSPACE_DIR}/README.md
COPY --chown=isaaclab:isaaclab ./settings.json ${WORKSPACE_DIR}/.vscode/settings.json

USER isaaclab:isaaclab

RUN code-server \
    --install-extension ms-python.python \
    --install-extension ms-toolsai.jupyter \
    --force ${WORKSPACE_DIR}

USER root

COPY entrypoint.sh /entrypoint.sh

# OV Cache Directories
RUN mkdir -p /root/.cache/ov
RUN mkdir -p /root/.local/share/ov

# Fix permissions for isaaclab user
RUN chown -R isaaclab:isaaclab /workspace /root/.cache/ov /root/.local/share/ov /isaac-sim/kit/cache

USER isaaclab:isaaclab

ENTRYPOINT ["/bin/bash"]

CMD ["/entrypoint.sh"]
