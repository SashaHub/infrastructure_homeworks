# 2 - Working with remote servers 

**git branch name:** jbrowser

## Theory \[2\]

-   \[0.4\] What are [computer
    ports](https://www.cloudflare.com/learning/network-layer/what-is-a-computer-port/)
    on a high level? How many ports are there on a typical computer?

Computer ports refer to specific channels through which data is
transmitted between different devices or computer systems on a network.
These ports are identified by unique numerical values, known as port
numbers, that allow different types of data to be transmitted through
them.

There are a total of 65536 available communication ports on a typical
computer, although only a small fraction of these are commonly used.
Some of the most common ports and their functions are:

Port 80 - HTTP (Hypertext Transfer Protocol)

Port 443 - HTTPS (Secure Hypertext Transfer Protocol)

Port 22 - SSH (Secure Shell)

Port 21 - FTP (File Transfer Protocol)

Port 25 - SMTP (Simple Mail Transfer Protocol)

Port 53 - DNS (Domain Name System)

Port 3389 - RDP (Remote Desktop Protocol)

Port 23 - Telnet (Teletype Network)

Port 554 - RTSP (Real-Time Streaming Protocol)

Port 110 - POP3 (Post Office Protocol version 3)

-   \[0.4\] What is the difference between http, https, ssh, and other
    protocols? In what sense are they similar? Name default ports for
    several data transfer protocols.

HTTP (Hypertext Transfer Protocol) is a protocol used for transferring
data on the internet. It is the foundation of data communication on the
web and is used to send and receive data between a web browser and a web
server.

HTTPS (Hypertext Transfer Protocol Secure) is a secure version of HTTP.
It uses a secure connection (SSL/TLS) to encrypt data transmitted
between a web browser and a web server.

SSH (Secure Shell) is a network protocol used for secure communication
between two devices. It is commonly used to remotely access and manage
servers, and to securely transfer files between systems.

So the difference between the protocols lies in the fact of encryption.
Protocols can also differ in their purpose of use.

Some examples include FTP (File Transfer Protocol) for transferring
files, SMTP (Simple Mail Transfer Protocol) for sending email, and POP
(Post Office Protocol) for receiving email.

The default port for HTTP is port 80, the default port for HTTPS is port
443, and the default port for SSH is port 22. However, these ports can
be changed if needed.

The most famous are port 80 for http, 443 for https and 20/21 for FTP.

-   \[0.4\] Explain briefly: (1) what is IP, (2) what IPs are called
    \'white\'/public, (3) and what happens when you enter \'google.com\'
    into the web browser.

IP stands for Internet Protocol. It is a protocol used for communication
on the internet. An IP address is a numerical label assigned to each
device connected to a computer network that uses the Internet Protocol
for communication.

IPs that are publicly available and can be accessed over the internet
are called \"white\" or \"public\" IPs. These IPs are assigned to
devices that are directly connected to the internet, such as servers and
routers.

When you enter \'google.com\' into a web browser, the following process
occurs:

The browser sends a request to a domain name server (DNS) to resolve the
domain name to an IP address. The DNS server responds with the IP
address of the server where the website is hosted. The browser sends a
request to the server using the IP address. The server responds by
sending the website\'s HTML, CSS, and JavaScript files to the browser.
The browser uses these files to render the website and display it to the
user.

-   \[0.4\] What is Nginx? How does it work on the high level? List
    several alternative web servers.

Nginx is an open-source web server that is used to host websites and web
applications.

On a high level, Nginx works by accepting incoming HTTP requests from
clients (such as web browsers) and forwarding them to a designated
backend server. The backend server then processes the request and
returns the appropriate response, which Nginx then sends back to the
client.

Nginx can be used as a standalone web server or as a reverse proxy,
where it sits in front of multiple backend servers and distributes
incoming requests between them.

Some alternative web servers include Apache, IIS (Internet Information
Services), and LiteSpeed.

-   \[0.4\] What is SSH, and for what is it typically used? Explain two
    ways to authenticate in an SSH server in detail. SSH (Secure Shell)
    is a network protocol that allows secure remote access to a computer
    or server over an unsecured network. It is typically used to
    remotely access and manage servers, as well as to securely transfer
    files between systems.

There are two main ways to authenticate in an SSH server:

Password-based authentication: This is the most common method of
authentication in SSH. It involves the user entering a correct password
in order to gain access to the server. The password is typically stored
in a file called the \"authorized_keys\" file on the server, which is
encrypted and only accessible to the server administrator.

Public key authentication: This method of authentication involves the
use of a pair of cryptographic keys - a public key and a private key -
to authenticate the user. The user\'s public key is stored on the
server, and the private key is kept on the user\'s local machine. When
the user attempts to connect to the server, the server sends a challenge
message that the user\'s private key can decrypt. If the decryption is
successful, the user is granted access to the server.

Both of these authentication methods are secure, but public key
authentication is generally considered more secure as it does not rely
on the user remembering a password, which can be easily compromised or
forgotten.

## Problem \[6.5\] {#problem-65}

A real-life situation that occurred to me several times over the years.

Imagine wrapping up a large bioinformatics project and wanting to share
raw data with your colleagues in a friendly and straightforward format.
The best option would be to use an online genome browser and host your
data remotely, so it is easily accessible by anyone with a valid link.
This is exactly what we will be doing here.

*Please consider doing this HW using Linux since setting up the SSH
client on Windows is painful, and I won\'t be able to help you.*

Steps:

-   \[2\] Create a new virtual machine in the Yandex/Mail/etc cloud
    (order at least 10GB of free disk space). Generate SSH key pair and
    use it to connect to your server.
-   \[1\] Download the latest human genome assembly (GRCh38) from the
    Ensemble FTP server
    ([fasta](https://ftp.ensembl.org/pub/release-108/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz),
    [GFF3](https://ftp.ensembl.org/pub/release-108/gff3/homo_sapiens/Homo_sapiens.GRCh38.108.gff3.gz)).
    Index the fasta using samtools (`samtools faidx`) and GFF3 using
    tabix.
-   \[1\] Select and download BED files for three ChIP-seq and one
    ATAC-seq experiment from the ENCODE (use one tissue/cell line).
    Sort, bgzip, and index them using tabix.
-   \[1\] Download and install [JBrowse 2](https://jbrowse.org/jb2/).
    Create a new jbrowse
    [repository](https://jbrowse.org/jb2/docs/cli/#jbrowse-create-localpath)
    in `/mnt/JBrowse/` (or some other folder).
-   \[0.25\] Install nginx and amend its config(/etc/nginx/nginx.conf)
    to contain the following section:

```{=html}
<!-- -->
```
    http {
      # Don't touch other options!
      # ........
      # ........

      # Add this:
      server {
    		listen 80;
    		index index.html;

    		location /jbrowse/ {
    			alias /mnt/JBrowse/;	
    		}
    	}

      # ........
    }

-   \[0.25\] Restart the nginx (reload its config) and make sure that
    you can access the browser using a link like this:
    `http://64.129.58.13/jbrowse/`. Here `64.129.58.13` is your public
    IP address.
-   \[1\] Add your files to the genome browser and verify that
    everything works as intended. Don\'t forget to
    [index](https://jbrowse.org/jb2/docs/cli/#jbrowse-text-index) the
    genome annotation, so you could later search by gene names.

**Remember to put a persistent link to a session with all your BED files
and the genome annotation in the report. I must be able to access it
without problems.**

### Create a VM

I created VM via Yandex Cloud. I used vCPU=2 and 100% performance, RAM=8Gb, Ubuntu
20.04 and HDD=30GB

    ssh-keygen -t ed25519

Here is how key looks like

    ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGmahvaQNMri07LxawD01Fr03blKtRBqNPi7SNVcYObc sanek@LAPTOP-82CNELG8

Connect with

    ssh -i hwkey alkirdeev@51.250.91.125 

Install everything that we will be needed in SSH server

    sudo apt-get install wget gunzip samtools tabix

### The latest human genome assembly (GRCh38)

I downloaded fa file and gff3 for hg38 genome. And index then via
samtools and tabix

    mkdir genome_assembly
    cd genome_assembly
    wget ftp://ftp.ensembl.org/pub/release-99/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
    gunzip Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
    samtools faidx Homo_sapiens.GRCh38.dna.primary_assembly.fa
    wget ftp://ftp.ensembl.org/pub/release-99/gff3/homo_sapiens/Homo_sapiens.GRCh38.99.gff3.gz
    gunzip Homo_sapiens.GRCh38.99.gff3.gz
    sort -k1,1 -k4,4n Homo_sapiens.GRCh38.99.gff3 > sorted.gff3
    bgzip sorted.gff3
    tabix -p gff sorted.gff3.gz
    сd ..

### Download BED and ATAC-seq files. Sort, bgzip, and index them using
tabix

→ Tissue - heart

-   ATAC-seq \-- ENCSR286STX
    <https://www.encodeproject.org/experiments/ENCSR286STX/>

-   CHIP-seq (conservative IDR thresholded peaks):

    1.  ENCSR350GZX (H3K4me1)
        <https://www.encodeproject.org/experiments/ENCSR350GZX/>

    H3K4me1 is an epigenetic modification to the DNA packaging protein
    Histone H3. It is a mark that indicates the mono-methylation at the
    4th lysine residue of the histone H3 protein and often associated
    with gene enhancers.

    1.  ENCSR433LHD (H3K9me3)
        <https://www.encodeproject.org/experiments/ENCSR433LHD/>

H3K9me3 is an epigenetic modification to the DNA packaging protein
Histone H3. It is a mark that indicates the tri-methylation at the 9th
lysine residue of the histone H3 protein and is often associated with
heterochromatin.

1.  ENCSR236KOX (H3K4me3)
    <https://www.encodeproject.org/experiments/ENCSR236KOX/>

H3K4me3 is an epigenetic modification to the DNA packaging protein
Histone H3 that indicates tri-methylation at the 4th lysine residue of
the histone H3 protein and is often involved in the regulation of gene
expression.The name denotes the addition of three methyl groups
(trimethylation) to the lysine 4 on the histone H3 protein.

H3 is used to package DNA in eukaryotic cells (including human cells),
and modifications to the histone alter the accessibility of genes for
transcription. H3K4me3 is commonly associated with the activation of
transcription of nearby genes. H3K4 trimethylation regulates gene
expression through chromatin remodeling by the NURF complex.This makes
the DNA in the chromatin more accessible for transcription factors,
allowing the genes to be transcribed and expressed in the cell. More
specifically, H3K4me3 is found to positively regulate transcription by
bringing histone acetylases and nucleosome remodelling enzymes
(NURF)H3K4me3 also plays an important role in the genetic regulation of
stem cell potency and lineage.This is because this histone modification
is more-so found in areas of the DNA that are associated with
development and establishing cell identity.

    wget https://www.encodeproject.org/files/ENCFF135EYC/@@download/ENCFF135EYC.bed.gz -O atac.bed.gz
    wget https://www.encodeproject.org/files/ENCFF947SST/@@download/ENCFF947SST.bed.gz -O chip1.bed.gz
    wget https://www.encodeproject.org/files/ENCFF144UHV/@@download/ENCFF144UHV.bed.gz -O chip2.bed.gz
    wget https://www.encodeproject.org/files/ENCFF539IOI/@@download/ENCFF539IOI.bed.gz -O chip3.bed.gz

    mkdir bed_files
    cd bed_files
    gunzip *.bed.gz
    for file in *.bed; do
    >     sort -k1,1 -k2,2n $file > "sorted_$file"
    >
    > done
    bgzip sorted_*.bed

     for file in sorted_*.bed.gz; do
    >     tabix -p bed $file
    > done

### Install JBrowse 2

I decided to install jbrowse via miniconda

    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    bash Miniconda3-latest-Linux-x86_64.sh
    miniconda3/bin/conda install -c bioconda jbrowse2

### Install Nginx

    sudo apt-get install -y nginx
    sudo nano /etc/nginx/nginx.conf

I changed conf file by adding server and commenting the line below.

      #include /etc/nginx/sites-enabled/*

      server {
        listen 80 default_server;
        index index.html;
        server_name _;

        # Don't put JBrowse inside the home directory!
        # You will have problems with permissions
        location /jbrowse/ {
          alias /home/amikhailova/JBrowse/;	
        }
      }

Then we should reload nginx and see that our jbrowse is working! Hurray!

    sudo nginx -s reload
    http://51.250.91.125/jbrowse/

### Add (BED & FASTA & GFF3) to genome browser {#add-bed--fasta--gff3-to-genome-browser}

Changed it couple of times, so right tracks are with \_new endings

    sudo /home/alkirdeev/miniconda3/bin/jbrowse add-assembly Homo_sapiens.GRCh38.dna.primary_assembly.fa --type indexedFasta --load copy --out /var/www/html/jbrowse/

    sudo /home/alkirdeev/miniconda3/bin/jbrowse text-index --file=/home/alkirdeev/genome_assembly/sorted.gff3.gz

    for i in chip1 chip2 chip3 atac
    do
      gunzip sorted_${i}.bed.gz
      awk '{gsub(/^chr/,""); print}' sorted_${i}.bed > $(echo sorted_${i}.bed| cut -d '.' -f 1)'_new.bed'
      bgzip sorted_${i}_new.bed
      tabix -f sorted_${i}_new.bed.gz
    done

    for i in chip1 chip2 chip3 atac
    do 
      sudo /home/alkirdeev/miniconda3/bin/jbrowse add-track sorted_${i}_new.bed.gz --load copy --out /var/www/html/jbrowse
    done

## Extra points \[1.5\] {#extra-points-15}

-   \[1\] Create a Docker container for running JBrowse 2. It should be
    a self-contained application, listening on the default HTTP port.
    Users must be able to mount directories with custom configs and
    access them later without any problems.

Hint: to specify the config, use the config=PATH query parameter. E.g.
`http://64.129.58.13/jbrowse/?config=my_folder%2Fconfig.json` where
`my_folder%2Fconfig.json` is the
[escaped](https://en.wikipedia.org/wiki/Percent-encoding) path to the
config file.

    #install docker and get image from miniconda
    sudo apt-get install docker.io
    sudo systemctl start docker
    sudo docker pull continuumio/miniconda3

    #listen to 80 port
    sudo docker run -p 80:80 -it --name jbrowse2 continuumio/miniconda3 /bin/bash
    conda install -c bioconda jbrowse2

    #make directory
    mkdir my_folder
    cd my_folder

    #make config
    touch config.json

    #change config to add escaped path
    nano config.json

    "mounts": {
        "data": my_folder/data1
    }

    jbrowse --config my_folder/config.json

    http://51.250.91.125//jbrowse/?config=my_folder%2Fconfig.json

-   \[0.5\] Give an in-depth explanation of the OSI model and how the
    TCP/IP stack works. Don\'t copy-paste descriptions from the
    internet; paraphrase and shorten as much as possible (imagine
    writing a cheat sheet for yourself).

In general, the OSI model defines the structure of communication between
devices on the network, and the TCP/IP stack is a set of protocols that
implement this structure for communication over the Internet.

The OSI (Open Systems Interconnection) model is a conceptual model that
explains how communication occurs between two devices in a network. It
consists of seven layers, each of which serves a specific function in
the communication process:

Physical layer: This layer is concerned with the physical transmission
of data between devices. It deals with issues such as cable types and
the signaling used to transmit data.

Data link layer: This layer is responsible for ensuring that data is
delivered accurately between devices that are directly connected, such
as between two computers connected by a network cable. It performs error
checking and flow control to ensure that data is transmitted correctly.

Network layer: This layer is responsible for routing data between
devices that are not directly connected. It determines the most
efficient path for the data to take and routes it accordingly.

Transport layer: This layer is responsible for establishing,
maintaining, and terminating connections between devices. It also
provides flow control and error checking to ensure that data is
transmitted correctly.

Session layer: This layer is responsible for establishing, maintaining,
and terminating connections between applications on different devices.
It also manages the flow of data between the applications.

Presentation layer: This layer is responsible for translating data into
a form that can be understood by the application layer. It handles
issues such as data compression and encryption.

Application layer: This is the highest layer of the OSI model and is
responsible for providing services to the user. It includes the
applications and programs that allow users to interact with the network.

The TCP/IP (Transmission Control Protocol/Internet Protocol) stack is a
set of protocols that are used to transmit data over the Internet. It is
based on the OSI model and consists of four layers:

Network Interface layer: This layer corresponds to the physical and data
link layers of the OSI model and is responsible for sending and
receiving data at the physical level.

Internet layer: This layer corresponds to the network layer of the OSI
model and is responsible for routing data between devices.

Transport layer: This layer corresponds to the transport layer of the
OSI model and is responsible for establishing, maintaining, and
terminating connections between devices.

Application layer: This layer corresponds to the session, presentation,
and application layers of the OSI model and is responsible for providing
services to the user.

