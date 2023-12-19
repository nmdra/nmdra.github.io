---
title: "Note-Taking with Pandoc: A Comprehensive Guide"
date: 2023-09-25
lastmod: 2023-09-25
showtoc: true
ShowReadingTime: true
ShowShareButtons: false
ShowPostNavLinks: true
ShowBreadCrumbs: false
ShowCodeCopyButtons: true
editPost:
  URL: "https://github.com/nmdra/nmdra.github.io/tree/main/content"
  Text: "Suggest edit"
  appendFilePath: true
---

## Introduction


[Pandoc](https://pandoc.org/) stands as the universal document converter. In this guide, we'll walk you through how to make the most of Pandoc's capabilities, turning your note-taking process into a seamless and productive experience. Whether you're a student, a researcher, or simply someone who loves to jot down thoughts, Pandoc can be the versatile tool you've been looking for.The primary focus of this guide is harnessing the power of Pandoc to effortlessly convert Markdown into beautifully formatted PDFs.


(article image)

## Installation

You can install pandoc on Linux, MacOS, Windows & Docker. 

In this article, we'll primarily focus on installing **Pandoc via Docker** in a Linux environment, but the steps are mostly applicable to other operating systems as well.

> **Why Docker**   
\
    In other Pandoc installation methods, you often need to install numerous dependencies. For example, if you want to convert Markdown to PDF, you'd need to install the TexLive PDF engine. If any of these dependencies are missing, you might encounter cryptic errors like `! LaTeX Error: File ifxetex.sty not found.`   
\
    With **Pandoc/extra** docker image, you get a bundled package that includes Pandoc, LaTeX, and carefully chosen components like the Eisvogel template, Pandoc filters, and open-source fonts. (We'll delve into templates and filters later in this article.) 

### Install Pandoc via docker
To use Pandoc with Docker, you first need to install Docker. Here are the installation steps for various operating systems.

{{< details "Docker Installation" >}}

+ Arch Linux-Based Systems

1. Install Docker using the following command:
   ```bash
   $ sudo pacman -S docker
   ```

2. Start the Docker service:
   ```bash
   $ sudo systemctl start docker.service
   ```

3. Enable Docker to start on boot:
   ```bash
   $ sudo systemctl enable docker.service
   ```

After successfully installing Docker, you can verify the installation by running:

```bash
$ sudo docker run hello-world
```

If the installation is successful, you will see the following message.
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
..
```

If you encounter any issues during installation, you can refer to the [ArchWiki](https://wiki.archlinux.org/title/docker).

+ Install Docker on Ubuntu-Based Systems

For detailed instructions on installing Docker on Ubuntu-based systems, please refer to the official Docker documentation: [Docker Installation Guide for Ubuntu](https://docs.docker.com/engine/install/ubuntu/).

+ Install Docker on MacOS

To install Docker on MacOS, you can download and install Docker Desktop for Mac from the official Docker website.

{{< /details >}}
\
Once Docker is successfully installed, you can proceed with following steps to install Pandoc.

1. Pull the Pandoc/extra Docker image by running the following command:
   ```bash
   docker pull pandoc/extra
   ```

2. Once the installation is complete, you can use the following command to convert a Markdown file (test.md) to PDF.

   ```bash
   docker run --rm \
       --volume "$(pwd):/data" \
       --user $(id -u):$(id -g) \
       pandoc/extra test.md -o outfile.pdf
   ```
For command-line usage, you can create a shell alias like this:

```bash
alias pandock='docker run --rm -v "$(pwd):/data" -u $(id -u):$(id -g) pandoc/extra'
```


## Markdown

Markdown guide

## templates & filters

## Bash script & Alias

## Tips and tricks

yaml header Eisvogel example
boxes
notes
add image
hr

highlight

## references
