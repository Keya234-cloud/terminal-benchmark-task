# Allowlisted base image, pinned by digest (never :latest, never a bare tag).
   # python:3.13-slim-bookworm from the approved public.ecr.aws mirror.
   # If your .dynamo/ allowlist pins a different specific digest, use that one instead.
   FROM public.ecr.aws/docker/library/python:3.13-slim-bookworm@sha256:fcbd8dfc2605ba7c2eca646846c5e892b2931e41f6227985154a596f26ab8ed7

   # Test dependencies are baked into this single image and pinned to exact versions.
   # The same image is reused for the verifier run; tests/ installs nothing at verify time.
   RUN pip install --no-cache-dir pytest==8.4.1 pytest-json-ctrf==0.3.5

   WORKDIR /app

   # Input data only. No solution, hint, or ground-truth files are ever copied into the agent image.
   COPY access.log /app/access.log
