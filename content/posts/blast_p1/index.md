---
title: "Installing BLAST on Macos and downloading the database"
date: 2024-10-05T14:05:13+02:00
draft: true
math: true
---

BLAST (Basic Local Alignment Search Tool) is a widely tool for comparing nucleotide or protein sequences against databases. If you're working on macOS and need to use any of its tools, this guide will help you install BLAST, download necessary databases, and run BLAST searches _efficiently_.
## Installing 
### Manually

Installing BLAST manually gives you control over the installation process and allows you to install the latest version directly from NCBI.

1. **Download the BLAST Package:**

   - Visit the NCBI BLAST download page: [NCBI BLAST+ Download](https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/)
   - Download the latest macOS binary package
       1. For intel based mac typically the file name should be like  `ncbi-blast-x.xx.x+-x64-macosx.tar.gz`.
       2. For apple silicone based mac typically the file name should be like  `ncbi-blast-x.xx.x+-aarch64-macosx.tar.gz `.

with `x.xx.x` as the version. As the time of writing this guide the latest version is `2.16.0`.

```bash
cd ~/Downloads
curl -O https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.13.0+-x64-macosx.tar.gz
```

**Note:** Replace the URL with correct one for your platform.

2. **Extract the Package:**

Open Terminal and navigate to the download directory:

   ```bash
   cd ~/Downloads
   tar -zxvf ncbi-blast-2.16.0+-x64-macosx.tar.gz
   ```

3. **You can now use BLAST binaries in the `ncbi-blast-2.16.0+/bin` directory.**

```bash
cd ncbi-blast-2.16.0+/bin
./blastn -version
```

If everything is working you should see the BLAST version information.

```bash
blastn: 2.16.0+
Package: blast 2.16.0, build Jun 25 2024 11:53:51
```

4. **Making BLAST available anywhere**

Right now if you want to use BLAST, you have to be in the `ncbi-blast-2.16.0+/bin` directory. To make it accessible anywhere, we have two solutions. Either you move the binaries into where your shell ( terminal ) searches for binaries, or you can tell it to look somewhere else (where BLAST is installed).
I normally prefer the second optione because 

   1. It gives me more flexibility on where I can install and manage third party tools
   2. I can have multiple version of BLAST and switch between them in one command

The way to accomplish this is by adding the directory where BLAST is installed to the `PATH` environment variable in macOS. The `PATH` variable simply contains a list of directories that macos uses to seacrh for binaries.

To add a directory into this variable, simply locate where BLAST is installed, for my setup, I keep such tools in `~/third_party/blast/<version>/bin` so for version `2.16.0`, the full directory path would be `~/third_party/blast/2.16.0/bin`


```bash
export PATH="$HOME/third_party/blast/2.16.0/bin:$PATH"
```
**Note**: This applies only to your current SHELL session, if you open a new terminal, you'll have to execute this command again to make BLAST accessbile for this session.

To make it persistant across session, you'll have to add the command to `~/.bashrc` or `~/.zshrc`.



6. **Verify Installation:**

```bash
blastn -version
```

   You should see the BLAST version information.

### Via Homebrew

Homebrew simplifies software installation on macOS.


1. **Install Homebrew (if not already installed):**

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. **Install BLAST via Homebrew:**

```bash
brew install blast
```

3. **Verify Installation:**

```bash
blastn -version
```

## Downloading the Database


BLAST requires databases to compare your query sequences against. Their database is available [here](https://ftp.ncbi.nlm.nih.gov/blast/db/).

1. **Choose a Database:**

   - Common databases:
     - `nt`: Nucleotide collection. It is used for nucleotide sequence searches, so if you have a DNA or RNA sequence and want to find similar nucleotide sequences, you would use the nt database.
     - `nr`: Non-redundant protein sequences. It is used for protein sequence searches, so if you have a protein sequence and want to find similar proteins, you would use the nr database.
     - Specific organism databases

2. **Create a Directory for Databases:**

```bash
mkdir -p ~/blastdb
```

3. **Download Databases Using `update_blastdb.pl`:**

You can download the database manually from the link above the extract them. However, BLAST provides a script to automatically do it.
The script is called `update_blastdb.pl` and is included in the `ncbi-blast-2.16.0+/bin` directory. 
The `update_blastdb.pl` script is included with BLAST. Since you made the full `bin` directory available in your `PATH`, you can also run this script from anywhere.

```bash
update_blastdb.pl --decompress nt  ~/blastdb # for nt database
update_blastdb.pl --decompress nr  ~/blastdb # for nr database
```

*Note:* Replace `nt` with the database of your choice.
The problem is that the database is huge and you probably shouldn't download all of it in your mac if you have another option. Normally I have a small database locally that I use to test scripts and when i'm confident with the script, I run it on the server where the full database is available.

3.  **Download a partial database** 
You can download only one part of the database by specifying the part you want to download. For example, to download the first file of the `nt` or `nr` database, you can run the following command:

```bash
update_blastdb.pl --decompress nt.00  ~/blastdb # for nt database
```
or using curl 
```bash
curl -O https://ftp.ncbi.nlm.nih.gov/blast/db/nr.00.tar.gz
tar -zxvf nr.00.tar.gz
```
**Note**: This file is 33GB in size before uncompressing, so make sure you have enough space on your mac.

4. **Set the `BLASTDB` Environment Variable:**
Like the terminal needs to know wher to search for binaries, BLAST needs to know where to search for databases. You can set the `BLASTDB` environment variable to the directory where you downloaded the databases.

```bash
export BLASTDB=~/blastdb
```

This also applies only to your current SHELL session, if you open a new terminal, you'll have to execute this command again to make BLASTDB accessbile to BLAST. To make it persistant across session, you'll have to add the command to `~/.bashrc` or `~/.zshrc`.

5. **Verify Database Files:**

```bash
ls ~/blastdb
```

## Running BLAST

With BLAST installed and databases downloaded, you can perform searches.

1. **Prepare Your Query Sequence File:**

Save your sequence in FASTA format (e.g., `query.fasta`).
You can try the below sequence as a test.

```bash
>Seq1
ATGGGTAAGGAGGACAAGACTCACCTTAACGT
CGTCGTCATCGGCCACGTCGACTCTGGCAAGT
CGACCACTGTAAGTACAACCAACAGCGGGTTG
CTTATCTGCACTCGGAATCCGCCAAACCTGGC
AGGGTATCACCAAAACATCTTGCTAACTTTTG
ACAGACCGGTCACTTGATCTACCAGTGCGGTG
GTATCGACAAGCGAACCATCGAGAAGTTCGAG
AAGGTTAGTCAATATCCCTTCGATTACGCGCG
CTCCCATCGATTCCCACGATTCGCTCCCTCAC
TCGAAACACATCCATTACCCCGCTCGAGTCCG
AAAATTTTGCGGTGCGACCGTGATTTTTTCTG
GTGGGGTATCTTACCCCGCCACTCGAGTCACG
GATGCGCTTGCCCTGTTCCCACAAAACCTTAC
CACCCTGTCGCGCACTACATGTCTTGCAGTCA
CTAACCACTGGACAATAGGAAGCCGCCGAGCT
CGGAAAGGGTTCCTTCAAGTACGCCTGGGTTC
TTGACAAGCTCAAAGCCGAGCGTGAGCGTGGT
ATCACCATTGATATCGCTCTCTGGAAGTTCGA
GACTCCTCGCTACTATGTCACCGTCATTGGTA
TGTTGTCACCGTCTCACACTATCATGTATTCA
TCATGCTAACATCTCTCTCAGATGCCCCCGGT
CATCGTGATTTCATCAAGAACATGATC 
```

2. **Run a BLAST Search:**

```bash
blastn -query query.fasta -db nt -out results.txt
```

   - `blastn`: Nucleotide-nucleotide BLAST
   - `-query`: Input file
   - `-db`: Database name
   - `-out`: Output file

3. **Customize Search Parameters (Optional):**

```bash
blastn -query query.fasta -db nt -out results.txt -evalue 1e-5 -outfmt 6 -max_target_seqs 10
```

   - `-evalue`: Expectation value threshold
   - `-outfmt 6`: Tabular output format
   - `-max_target_seqs`: Maximum number of aligned sequences to keep

**Note**: if you are getting any error related to the database, make sure that:

   1. `BLASTDB` environment variable is set correctly.
   2. The database is downloaded and decompressed correctly.
   3. The database is in the correct directory.
   4. You can run `blastdbcmd -db nt -info` to check if the database is correctly installed.
      -  Make sure you are using the correct database name in the command.
   5. Make sure you are using the correct command.
      - `blastn` for nucleotide-nucleotide BLAST
      - `blastp` for protein-protein BLAST




<br>

4. **View Results:**

```bash
less results.txt
```

## Conclusion

Installing BLAST on macOS is straightforward, whether you choose a manual installation or use Homebrew. With BLAST set up and databases downloaded, you're ready to perform powerful sequence analyses directly from your Mac. In the next guide, I'll go over how to interpret the results from BLAST.

## References

- [NCBI BLAST+ Download](https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/)
- [BLAST Command Line Applications User Manual](https://www.ncbi.nlm.nih.gov/books/NBK279690/)
- [Homebrew](https://brew.sh/)
- [NCBI BLAST Databases](https://ftp.ncbi.nlm.nih.gov/blast/db/)
