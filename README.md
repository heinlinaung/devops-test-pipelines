# TaskD — Answers

## How did you test your pipelines?
- Setup Jenkins locally (Dockerfile and plugins as below)
```
### Dockerfile
FROM jenkins/jenkins:lts-jdk11

USER root

RUN apt-get update && \
  apt-get install -y \
  doxygen \
  python3 \
  python3-pip \
  && apt-get clean && \
  rm -rf /var/lib/apt/lists/*

USER jenkins

## Jenkins plugins list
- Timestamper
- Workspace Cleanup
- Pipeline
- Git
```
- Set up two pipelines - PipelineB and PipelineC
- Triggered builds manually and watched console outputs.
  - Pipeline B
    - Verified that `doxygen` generated documentation and output artifact (doc.tar.gz)
  - Pipeline C
    - Checked that `log_parser.py` created `output.csv` as an artifact.

## How did you test repoC Python?
- Cloned `RepoC` locally.
- Created a sample `warnings.log` or use the generated with the RepoA.
- Run `python3 log_parser.py warnings.log`.
- Verified the creation and structure of `output.csv` and its file content.

## RepoA-doc contains binaries
### What is the advantage to use LFS?
- Git LFS stores large files outside the main Git repo.
- Keeps repository size small and improves clone/pull performance.
- Prevents Git history bloat.
- Seamlessly integrates with standard Git commands.
Reference: [Git LFS](https://git-lfs.github.com/)

## How to adjust this repository to support LFS?
1. Install Git LFS:

    ```bash
    git lfs install
    ```

2. Track file types:

    ```bash
    git lfs track "*.bin"
    git lfs track "*.pdf"
    git add .gitattributes
    ```

3. Add and push files normally:

    ```bash
    git add path/to/largefile.bin
    git commit -m "Add large files via LFS"
    git push
    ```

Useful links:
- [Git LFS Documentation](https://github.com/git-lfs/git-lfs)
- [GitHub Docs: Managing Large Files](https://docs.github.com/en/repositories/working-with-files/managing-large-files/configuring-git-large-file-storage)

## Are there other (easier) alternatives?
- **GitHub Releases**: upload binaries separately as release assets.
- **External storage (S3, GCS)**: keep large files outside GitHub.
- **CI/CD artifacts**: generate binaries during build, not stored in Git.