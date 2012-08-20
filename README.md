
FIRE-CBIR: Content-Based Image Retrieval
========================================

<a name="a" id="a">&nbsp;</a>

Acknowledgements
----------------

This is a fork, maintained by me, fish2000; it is based on [the original research work of Thomas Deselaers](http://thomas.deselaers.de/fire/), which he undertook as part of his [thesis work](http://thomas.deselaers.de/publications/papers/deselaers_diploma03.pdf) [PDF] at [a university
whose name I can't even dream of pronouncing without butchering it somehow](http://www.rwth-aachen.de). My copy of the codebase came from [the FIRE-CBIR Google Code repository](http://code.google.com/p/fire-cbir/).

Please see [Dr. Deselaers' FIRE-CBIR site](http://thomas.deselaers.de/fire/) for a full list of the many
talented contributors to this fine codebase. It is their wish that users of FIRE-CBIR will cite their work with this formal citation:

> Deselaers, T., Keysers, D., Ney, H.,  "Features for Image Retrieval: An Experimental Comparison",  Information Retrieval, vol. 11, issue 2, The Netherlands, Springer, pp. 77-107, 03/2008.

See [CITATIONS.txt](https://github.com/fish2000/fire-cbir/blob/master/CITATIONS.txt) for a complete bibliographical excerpt.

<a name="h" id="h">&nbsp;</a>

How To Install FIRE-CBIR: A Step-By-Step Guide
==============================================

*Adapted from http://code.google.com/p/fire-cbir/wiki/FIREUbuntu104*


<a name="i" id="i">&nbsp;</a>

Integration with Apache (CGI)
-----------------------------

Execute these commands, setting `$FIRE_HOME` to the root path of your un-tarballed local copy of FIRE-CBIR and `$CGI_BIN` to your Apache installation's `cgi-bin` directory.

* `apt-get install apache2  # if necessary`
* `cd $FIRE_HOME`
* `sudo cp ${FIRE_HOME}/Python/firesocket.py $CGI_BIN`
* `cd ${FIRE_HOME}/WebInterface`
* `cp ./fire.py ./feature.py ./img.py ./config.py ./fire-template.html $CGI_BIN`
* `mkdir ${CGI_BIN}/images  # if necessary`
* `cp ./neutral.png ./positive.png ./negative.png ./fire-logo.png ./i6.png $CGI_BIN`

Now all the files should be in place for the FIRE webinterface to work.


<a name="d" id="d">&nbsp;</a>

Setting up a dataset
--------------------


1. Create a directory, e.g.
    - `export FIRE_IMAGE_DATA="~/fire-image-data"`
    - `mkdir -p ${FIRE_IMAGE_DATA}`



2. Put a set of medium sized images (e.g. 500x500 pixels) into the `$FIRE_IMAGE_DATA` directory.
To start, try using around 100 images.



3. Convert all images to the JPG format
    - This step is not strictly necessary, but it often helps to avoid problems
    - `cd $FIRE_IMAGE_DATA && mogrify -format jpg *`
    - The `mogrify` command is part of [ImageMagick](http://www.imagemagick.org/index.php), a
    separate package. It can be freely installed if necessary:
        + `apt-get install imagemagick imagemagick-dev` or
        + `brew -v install imagemagick` will provide it.



4. Use FIRE-CBIR to extract color histograms from your images
    - `${FIRE_HOME}/bin/extractcolorhistograms --color --images ${FIRE_IMAGE_DATA}/*.jpg`



5. create a filelist for this dataset
    - `echo FIRE_filelist > ${FIRE_IMAGE_DATA}/filelist`
    - `echo suffix color.histo.gz >> ${FIRE_IMAGE_DATA}/filelist`
    - `echo path this >> ${FIRE_IMAGE_DATA}/filelist`
    - `ls ${FIRE_IMAGE_DATA}/*.jpg | sed 's/^/file /' >> filelist`

Now you have a dataset where every image is represented using a color
histogram. Other features can be added by extracting them and adding the
corresponding suffix lines to the filelist.


<a name="f" id="f">&nbsp;</a>

Start the FIRE web server †
---------------------------

To access FIRE through its built-in web interface:

* `${FIRE_HOME}/bin/fire -f ${FIRE_IMAGE_DATA}/filelist -r 10`
* Access FIRE's web interface via [http://localhost/cgi-bin/fire.py](http://localhost/cgi-bin/fire.py)
* From there, you should be able to click on an image to see similar images, based on their respective color histograms.


<a name="ff" id="ff">&nbsp;</a>

Comments ††
-----------

*Comment by chenkai0...@hotmail.com ,  Jul 27, 2010*

Hi Thomas,
Thanks for this doc. It is clear. With it I have succeed in installing FIRE.
But there are some bugs in the `fire.py`. After access the
webinterface through http://localhost/cgi-bin/fire.py, I click the upload form,
then I get the message error in the page as follow:

    Traceback (most recent call last):
        File "/usr/lib/cgi-bin/fire.py",
            line 862, in message+=newFile(form)
        File "/usr/lib/cgi-bin/fire.py",
            line 359, in newFile
        temporaryFiles=config.tempdir # standard: "/tmp" AttributeError?:
            `module` object has no attribute `tempdir`

Do you have some ideas about this ?
Best Kai


*Comment by bupt.z...@gmail.com ,  Aug 2, 2010*

I have the same problem with the above person.If you have same idea about it
and have free time,please tell me. thank you!


*Comment by xwj....@gmail.com ,  Aug 24, 2010*

hi guys, in `config.py`,u should delete `#` in front of
`tempdir="/tmp"` in line 12; also, u may add a line
`featureurl="feature.py"` to keep the system running, but still not
sovling the problem, seems something else to be done to run the uploading
function.
i am new to this system. some questions: the page can not display some of the
PNG pics, `negative.png, positive.png,...`, but logo.png is ok...i
dont know why. can anybody help,plz.
and it can not display the histogram image, a PNG file again, i guess....
regards


*Comment by Queenmar...@gmail.com ,  Aug 31, 2010*

I click the upload form, then I get the message error in the page as follow:
Traceback (most recent call last): File "/usr/lib/cgi-bin/fire.py", line 862,
in message+=newFile(form) File "/usr/lib/cgi-bin/fire.py", line 359, in newFile
temporaryFiles=config.tempdir # standard: "/tmp" AttributeError??:
`module` object has no attribute `tempdir` plz help me
because this important for me


*Comment by Dietrich...@gmail.com ,  Feb 4, 2011*

If your pictures are not displayed at the localhost webinterface, make sure
that your webserver has the proper rights to access your pictures.
Another hint, to make sure that FIRE really finds similar pictures, the proper
command for launching the server should be:

    ./bin/fire -f ~/fire-img/filelist -D -r 10

Regards


*Comment by shindino...@gmail.com ,  Mar 23, 2011*

The negative.png is spelled wrong. it is negativ.png, but i still cannot access
the localhost webinterface. The terminal ends with: ... (10) Retriever/
server.cpp:366:parseConfig? result=10 (10) Retriever/server.cpp:972:start? No
proxy (2) Retriever/server.cpp:997:start? Waiting for connections on port 12960

so I changed the localhost to that port via localhost:12960/cgi-bin/fire.py but
not getting anything


*Comment by fdewe...@gmail.com ,  Jun 8, 2011*

I have exactly the same problem, have you found where the problem is?
Thank!
Best


*Comment by hustling...@gmail.com ,  Jun 9, 2011*

Has anyone solved this problem?
Thanks!
Regards


*Comment by zhili...@gmail.com ,  Jul 21, 2011*

How to make sure my webserver has the proper rights to access the pictures?
Thanks!
Regards


*Comment by burningn...@gmail.com ,  Nov 30, 2011*

hi I got an Assertion failure when executing the following cmd:

    ~/fire-cbir/bin/extractcolorhistograms --color --images ~/fire-img/.jpg

The Error msg is: (10) [FeatureExtractors/extractcolorhistogram.cpp:93:main]
Processing `/home/zxj/fire-img/001.jpg` (1/101).
extractcolorhistogram: magick/semaphore.c:525: LockSemaphoreInfo?: Assertion
`semaphore_info != (SemaphoreInfo? ) ((void)0)` failed. Aborted


*Comment by burningn...@gmail.com ,  Dec 1, 2011*

It seems something wrong with the ImageMagcik?. Anyone know how to fix this
issue?


*Comment by zbzhengb...@gmail.com ,  Apr 9, 2012*

hi,guys,I come across a problem when I compile FIRE using the cmd "make":

    /usr/bin/ld: lib//libRetriever.a(retriever.o): undefined reference to symbol `omp_get_num_threads@@OMP_1.0`
    /usr/bin/ld: note: 'omp_get_num_threads@@OMP_1.0' is defined in DSO /usr/lib/libgomp.so.1

so try adding it to the linker command line /usr/lib/libgomp.so.1:

    could not read symbols: Invalid operation collect2: error: ld returned 1 exit
    status make: [bin//fire] Error 1

my os is ARCH 32bit.Google says that because my os is 32bit while the program
should be compiled on 64bit os.I don`t know if it is true.Anyone who have
compiled it can help me?


<small>
Editor's notes:

<a name="t" id="t">&nbsp;</a>

† I am pretty confident that [the late mod_python](http://www.modpython.org), or something like it,
is needed in order to deploy Python code by plopping some of it into a CGI directory, as per the
FIRE-CBIR author's directive. I don't know about that stuff (I'm more of a WSGI gunicorn-y type guy,
personally) but I'd wager you'll investigate that, like as like a part of the same content-based
journey through the forgotten world of image retrieval, upon which you and I are both apparently adrift.
<a href="#f">§ BACK TO TEXT</a>

<a name="tt" id="tt">&nbsp;</a>

†† These were provided by google code users. I cleaned them up a little bit but it was all
[like that when I got here](http://code.google.com/p/fire-cbir/wiki/FIREUbuntu104), basically.
<a href="#ff">§ BACK TO TEXT</a>


</small>
