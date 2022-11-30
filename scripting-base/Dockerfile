ARG BASE_IMAGE=ubuntu:jammy

FROM $BASE_IMAGE AS base

	RUN set -x && \
		apt-get update && \
		apt-get install -y \
			jq curl git ca-certificates && \
		rm -rf /var/lib/apt/lists/*


FROM base AS imgpkg

	ARG IMGPKG_CHECKSUM=bfc210872c18fd45e8012aadba27588f51c53cb237e397439abe5fe999e3a6fd
	ARG IMGPKG_VERSION=0.32.0

	RUN set -eux && \
		url=https://github.com/vmware-tanzu/carvel-imgpkg/releases/download/v${IMGPKG_VERSION}/imgpkg-linux-amd64 ; \
			curl -sSL $url -o /usr/local/bin/imgpkg && \
		echo "${IMGPKG_CHECKSUM}  /usr/local/bin/imgpkg" | sha256sum -c && \
		chmod +x /usr/local/bin/imgpkg

FROM base AS kbld

	ARG KBLD_CHECKSUM=bbae1d86b627b1a78cf9d0e1b911377fa55f2dbc058b964cdada8382bf6432dd
	ARG KBLD_VERSION=0.36.0

	RUN set -eux && \
		url=https://github.com/vmware-tanzu/carvel-kbld/releases/download/v${KBLD_VERSION}/kbld-linux-amd64 ; \
			curl -sSL $url -o /usr/local/bin/kbld && \
		echo "${KBLD_CHECKSUM}  /usr/local/bin/kbld" | sha256sum -c && \
		chmod +x /usr/local/bin/kbld

FROM base AS ytt

	ARG YTT_CHECKSUM=b3fbce9c6828c7eea09491c24fe49ddba7afe09e4405db33373d2776c91b1e6c
	ARG YTT_VERSION=0.44.0

	RUN set -eux && \
		url=https://github.com/vmware-tanzu/carvel-ytt/releases/download/v${YTT_VERSION}/ytt-linux-amd64 ; \
			curl -sSL $url -o /usr/local/bin/ytt && \
		echo "${YTT_CHECKSUM}  /usr/local/bin/ytt" | sha256sum -c && \
		chmod +x /usr/local/bin/ytt

FROM base AS jx-scm

	ARG JXSCM_VERSION=0.1.5

	RUN set -eux && \
		curl -L https://github.com/jenkins-x-plugins/jx-scm/releases/download/v0.1.5/jx-scm-linux-amd64.tar.gz | tar xzv && \
		mv jx-scm /usr/local/bin


FROM base AS final

	COPY --from=jx-scm 	/usr/local/bin/jx-scm	/usr/local/bin/jx-scm
	COPY --from=imgpkg 	/usr/local/bin/imgpkg	/usr/local/bin/imgpkg
	COPY --from=kbld 	/usr/local/bin/kbld		/usr/local/bin/kbld
	COPY --from=ytt 	/usr/local/bin/ytt		/usr/local/bin/ytt
