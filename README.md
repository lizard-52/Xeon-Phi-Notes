# Xeon-Phi-Notes
 
This is a repository to keep track of what things I've tried with my Xeon Phi 5110p.

## Should You Buy a Xeon Phi?
Not unless you like cross-compiling things and fighting with all sorts of software. The cards are so old now that a decent CPU will generally be faster, even in programs that are very favorable to Xeon Phi.

## Compatibility:

Here's my setup:

<b>CPU:</b>         R7 3700x

<b>Motherboard:</b> MSI X370 Gaming Titanium, AGESA 1.2.0.7

<b>RAM:</b>         2x8GB Samsung B-Die

<b>OS:</b>          Windows 10, Ubuntu 14.10 VM in VMware


The Xeon Phi works in all the slots it fits in as long as “Above 4G Decoding” is on. It does not work properly if I install an Optane SSD in the same computer. While not officially supported on pretty much any modern motherboard Phis seem to work on all platforms with "Above 4G Decoding". 

## Getting Started

When the 5110p came out using it with Windows was the <a href=https://www.pugetsystems.com/labs/hpc/Windows-10-with-Xeon-Phi-733/>worst option</a>. Now things are different and Windows 7/10 is really the best option, as getting really old RHEL or openSUSE working is a huge pain. The driver package (and assorted utilities) for Xeon Phi is called MPSS. You can still get it from Intel's website <a href=https://www.intel.com/content/www/us/en/developer/articles/tool/manycore-platform-software-stack-mpss.html>here</a>. You'll also need an old (version 14.2 works) Intel OpenCL runtime if you want to use OpenCL.

It's important to remember that a Xeon Phi coprocessor is pretty much it's own computer. This means you'll want to SSH into it. The IP address can be viewed/changed at C:\Program Files\Intel\MPSS\mic0.xml, assuming you have one Xeon Phi and installed to the default location.

Once you have a card running properly (try micctrl -s, it should say "mic0: online") have a look at some of the other folders here where I've done my best to document the things I've done.

## Some Important Words/Acronyms/Phrases
<b>MPSS</b> "Manycore Platform Software Stack" Driver and utilities for Xeon Phi coprocessors. Version 3.8.6 is the best for first gen (most) coprocessors.

<b>mic</b> "Many Integrated Core"

<b>k1om</b> Instruction set name for KNL CPUs. It is similar to x86, but missing some instructions that even the original 8086 has. No (non trivial) code compiled for x86 will run on k1om targets.

<b>KNL</b> "Knight's Landing" Intel codename for first gen (3100, 5100, and 7100 series) Xeon Phi.

<b>KNF</b> "Knight's Ferry" Intel codename for "zeroth" gen Xeon Phi. Pretty much same hardware as Larrabee and never properly released.

<b>Larrabee</b> Codename for Intel's second go at making graphics cards. Was canceled after hardware and software was working. A few cards escaped Intel labs, but drivers are still MIA.  

## Possibly Useful Links
Guide to getting cross-compiling setup:
https://aidancrowther.com/project/xeonphi


Blog with information about the predecessor to KNL:
https://knfarchive.blogspot.com/

Xeon Phi/MPSS guide:
https://registrationcenter.intel.com/irc_nas/11523/mpss_users_guide-windows.pdf