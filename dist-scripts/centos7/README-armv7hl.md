How to compile, install and configure GMediaRender for Centos 7 on armv7hl
--------------------------------------------------

This guide has been tested on a Raspberry Pi 2. The main challenge to get GMediaRender running on Centos 7 for arm is the non-availability of the required packages. Luckily, the source RPMs are available and generally also compile on Centos for arm. If you are impatient, skip to the section "Install pre-requisite packages (the shortcut)" and download the required rpms from http://www.bluhm-de.com[complete link].

To build the pre-requisite packages for GMediaRender, a lot of other packages need to be build and processed down all the dependency paths. Whilst this looks like a lof of steps, is it mainly working down the order and being patient. Compiling everything will take hours and you will definetly not finish it in one morning/night. This is assuming you are compiling directly on the Raspberry Pi 2.

The next section is organised in a hierachical and non linear way. The intention is to show depenencies and bundle pre-requisites. To separate the steps, here is a schema:

1. Download Source A
  1.1 Download Source B for Package A
  1.1 Compile Package B
  1.2 Download Source C for Package A
  1.2 Compile Package C
  1.3 Download Source N for Package A
  1.3 Compile Package N
2. Install all dependencies for A including Package B, C and N
3. Compile Package A

Levels will also be added for all dependencies of the second level...

wget http://li.nux.ro/download/nux/dextop/el7/SRPMS/gpac-0.5.1-14.20141206svn.el7.nux.src.rpm

yumdownloader --source gtk2			// done
yum install atk-devel libtiff-devel cups-devel libXcursor-devel libXinerama-devel libXcomposite-devel
rpmbuild --rebuild gtk2*			// done
yumdownloader --source graphviz
// wget http://www.graphviz.org/pub/graphviz/stable/SRPMS/graphviz-2.40.1-1.src.rpm
yumdownloader --source libgnomeui		// done
yumdownloader --source gnome-vfs2		// done
yum -y install GConf2-devel ORBit2-devel libsmbclient-devel gamin-devel avahi-glib-devel dbus-devel dbus-glib-devel libacl-devel
rpmbuild --rebuild gnome-vfs2*			// done
yumdownloader --source libgnomecanvas		// done
yumdownloader --source libglade2		// done
yum install ~/rpmbuild/RPMS/armv7hl/gtk2-devel* ~/rpmbuild/RPMS/armv7hl/gtk2-2*
rpmbuild --rebuild libglade2*			// done
yum -y install libart_lgpl-devel  ~/rpmbuild/RPMS/armv7hl/gtk2-devel* ~/rpmbuild/RPMS/armv7hl/libglade2-devel* ~/rpmbuild/RPMS/armv7hl/libglade2-2*
rpmbuild --rebuild libgnomecanvas*		// done
yumdownloader --source libgnome
yumdownloader --source libcanberra		// done
yumdownloader --source gtk3			// done
### not sure if all required for GTK3 INSTALLATION..... retest without
yumdownloader --source cairo // required if there is an alert for 32 bit versions
rpmbuild --rebuild cairo*			// done
yum reinstall ~/rpmbuild/RPMS/armv7hl/cairo-1* ~/rpmbuild/RPMS/armv7hl/cairo-devel*
yumdownloader --source pango // required if there is an alert for 32 bit versions
yum install libthai-devel help2man
rpmbuild --rebuild pango*			// done
yumdownloader --source libXrandr		// done
yumdownloader --source xorg-x11-util-macros	// done
rpmbuild --rebuild xorg-x11-util-macros*	// done
yum install -y ~/rpmbuild/RPMS/noarch/xorg-x11-util-macros-1.19.0-3.el7.noarch.rpm
rpmbuild --rebuild libXrandr*			// done
yum reinstall ~/rpmbuild/RPMS/armv7hl/pango-1* ~/rpmbuild/RPMS/armv7hl/pango-devel* ~/rpmbuild/RPMS/armv7hl/libXrandr-1*
yum -y install gnome-common at-spi2-atk-devel cairo-gobject-devel rest-devel json-glib-devel colord-devel avahi-gobject-devel
rpmbuild --rebuild gtk3*			// done
yum install -y systemd-devel libtdb-devel ~/rpmbuild/RPMS/armv7hl/gtk3-devel* ~/rpmbuild/RPMS/armv7hl/gtk3-3*
rpmbuild --rebuild libcanberra*			// done
yumdownloader --source gvfs			// done
yumdownloader --source gnome-online-accounts	// done
yumdownloader --source gcr			// done
yum install -y ~/rpmbuild/RPMS/armv7hl/gtk3-devel* libtasn1-tools vala-devel vala-tools valgrind-devel
rpmbuild --rebuild gcr*				// done
yumdownloader --source ruby			// done
yum install -y libyaml-devel readline-devel
rpmbuild --rebuild --nocheck ruby*		// done - Ruby tests are not successful! So use Ruby with care.
yumdownloader --source webkitgtk3		// done
yum install -y enchant-devel geoclue2-devel gperf libsecret-devel ~/rpmbuild/RPMS/armv7hl/gtk3-devel*  ~/rpmbuild/RPMS/armv7hl/gtk3-3* libwebp-devel \
libxslt-devel sqlite-devel perl-Switch ~/rpmbuild/RPMS/armv7hl/ruby-2*  ~/rpmbuild/RPMS/armv7hl/ruby-libs* ~/rpmbuild/RPMS/noarch/rubygems-2* \
~/rpmbuild/RPMS/armv7hl/rubygem-bigdecimal* ~/rpmbuild/RPMS/armv7hl/rubygem-io-console*  ~/rpmbuild/RPMS/armv7hl/rubygem-psych* \
~/rpmbuild/RPMS/noarch/rubygem-rdoc* ~/rpmbuild/RPMS/noarch/ruby-irb* ~/rpmbuild/RPMS/armv7hl/rubygem-json*
rpmbuild --rebuild webkitgtk3*			// fails due to assembly code
yum install -y https://armv7.dev.centos.org/repodir/c71511-pass-1/webkitgtk3/2.4.9-5.el7/armv7hl/libwebkit2gtk-2.4.9-5.el7.armv7hl.rpm https://armv7.dev.centos.org/repodir/c71511-pass-1/webkitgtk3/2.4.9-5.el7/armv7hl/webkitgtk3-2.4.9-5.el7.armv7hl.rpm https://armv7.dev.centos.org/repodir/c71511-pass-1/webkitgtk3/2.4.9-5.el7/armv7hl/webkitgtk3-devel-2.4.9-5.el7.armv7hl.rpm
yum install -y ~/rpmbuild/RPMS/armv7hl/gcr-devel* ~/rpmbuild/RPMS/armv7hl/gtk3-devel* ~/rpmbuild/RPMS/armv7hl/webkitgtk3-devel* libsecret-devel  \
	telepathy-glib-devel ~/rpmbuild/RPMS/armv7hl/webkitgtk3-2*
rpmbuild --rebuild gnome-online-accounts*	// done
yum install -y libcdio-paranoia-devel libgudev1-devel libsecret-devel libudisks2-devel libbluray-devel libxslt-devel fuse-devel libtalloc-devel libarchive-devel libgphoto2-devel libusb-devel libimobiledevice-devel libmtp-devel \
~/rpmbuild/RPMS/armv7hl/gnome-online-accounts-devel* ~/rpmbuild/RPMS/armv7hl/gtk3-devel*  ~/rpmbuild/RPMS/armv7hl/gnome-online-accounts-3*
rpmbuild --rebuild gvfs*			// done
yumdownloader --source glib2			// done
yum install -y elfutils-libelf-devel
rpmbuild --rebuild glib2*			// done
yum install -y libbonobo-devel libxslt-devel gnome-common popt-devel ~/rpmbuild/RPMS/armv7hl/gnome-vfs2-devel* ~/rpmbuild/RPMS/armv7hl/gnome-vfs2-2* \
~/rpmbuild/RPMS/armv7hl/gvfs-1* ~/rpmbuild/RPMS/armv7hl/glib2-2* ~/rpmbuild/RPMS/armv7hl/libcanberra-devel ~/rpmbuild/RPMS/armv7hl/libcanberra-gtk* \
~/rpmbuild/RPMS/armv7hl/gvfs-devel* ~/rpmbuild/RPMS/armv7hl/libcanberra-0*
rpmbuild --rebuild libgnome*			// done
yumdownloader --source libbonoboui		// done
yum install -y libbonobo-devel libgnomecanvas-devel libgnome-devel
rpmbuild --rebuild libbonoboui*			// done
yum install -y GConf2-devel libart_lgpl-devel libgnome-keyring-devel ~/rpmbuild/RPMS/armv7hl/gnome-vfs2-devel ~/rpmbuild/RPMS/armv7hl/gtk2-devel* \
~/rpmbuild/RPMS/armv7hl/libglade2-devel*  ~/rpmbuild/RPMS/armv7hl/libgnomecanvas-devel* ~/rpmbuild/RPMS/armv7hl/libbonoboui-devel* \
~/rpmbuild/RPMS/armv7hl/libgnome-devel*  ~/rpmbuild/RPMS/armv7hl/libgnomecanvas-2* ~/rpmbuild/RPMS/armv7hl/libgnome-2* \
~/rpmbuild/RPMS/armv7hl/gvfs-devel* ~/rpmbuild/RPMS/armv7hl/libbonoboui-2*
rpmbuild --rebuild libgnomeui* 			//done

gts-devel 
wget http://www.graphviz.org/pub/graphviz/stable/SRPMS/gts-0.7.6-21.20111025.fc18.src.rpm


yum install -y ksh tk-devel tcl-devel libtool-ltdl-devel  guile-devel libXaw-devel java-devel php-devel lua-devel qpdf ocaml perl-ExtUtils-Embed ghostscript-devel \
gd-devel ~/rpmbuild/RPMS/armv7hl/ruby-2*  ~/rpmbuild/RPMS/armv7hl/ruby-libs* ~/rpmbuild/RPMS/noarch/rubygems-2* \
~/rpmbuild/RPMS/armv7hl/rubygem-bigdecimal* ~/rpmbuild/RPMS/armv7hl/rubygem-io-console*  ~/rpmbuild/RPMS/armv7hl/rubygem-psych* \
~/rpmbuild/RPMS/noarch/rubygem-rdoc* ~/rpmbuild/RPMS/noarch/ruby-irb* ~/rpmbuild/RPMS/armv7hl/rubygem-json*  ~/rpmbuild/RPMS/armv7hl/ruby-devel*
yum install ~/rpmbuild/RPMS/armv7hl/libgnomeui-devel* ~/rpmbuild/RPMS/armv7hl/libgnomeui-2* 
yum install ann-devel gtkglarea2-devel gtkglext-devel poppler-glib-devel qt-devel ~/rpmbuild/RPMS/armv7hl/gts-devel* \
~/rpmbuild/RPMS/armv7hl/gts-0* --nogpgcheck
rpmbuild -bp graphviz*				// done
cp ~/rpmbuild/SPECS/graphviz.spec .
sed s/RUBY=1/RUBY=0/g graphviz.spec
rpmbuild --rebuild graphviz*			// done
wget -------- gstreamer-plugins-base*
yum --rebuild gstreamer-plugins-base*		// done
yum install -y ImageMagick freeglut-devel js-devel openjpeg-devel libXpm-devel wxGTK-devel xmlrpc-c-devel doxygen --nogpgcheck
wget http://li.nux.ro/download/nux/dextop/el7/SRPMS/xvidcore-1.3.2-5.el7.nux.src.rpm
rpmbuild --rebuild xvidcore*			// done
wget http://li.nux.ro/download/nux/dextop/el7/SRPMS/faad2-2.7-5.el7.nux.src.rpm
yum install id3lib-devel libsysfs-devel xmms-devel --nogpgcheck
rpmbuild --rebuild faad2*			// done
yum install -y ~/rpmbuild/RPMS/armv7hl/faad2-devel* ~/rpmbuild/RPMS/armv7hl/xvidcore-devel* ~/rpmbuild/RPMS/armv7hl/faad2-libs* ~/rpmbuild/RPMS/armv7hl/faad2-2*  \
~/rpmbuild/RPMS/armv7hl/graphviz-2*  ~/rpmbuild/RPMS/armv7hl/graphviz-nox*  ~/rpmbuild/RPMS/armv7hl/graphviz-x* ~/rpmbuild/RPMS/armv7hl/graphviz-plugins-core* \
~/rpmbuild/RPMS/armv7hl/graphviz-libs*  ~/rpmbuild/RPMS/armv7hl/graphviz-plugins-x*  
rpmbuild --rebuild gpac-0.5.1-14.20141206svn.el7.nux.src.rpm
wget http://li.nux.ro/download/nux/dextop/el7/SRPMS/ffmpeg-2.6.8-3.el7.nux.src.rpm
wget http://li.nux.ro/download/nux/dextop/el7/SRPMS/faac-1.28-6.0.el7.nux.src.rpm
yum install libmp4v2-devel --nogpgcheck
rpmbuild --rebuild faac*			// done
wget http://ftp.videolan.org/pub/videolan/x265/x265_2.3.tar.gz  -O ~/rpmbuild/SOURCES/x265_2.3.tar.gz
yum -y install cmake yasm --nogpgcheck
wget https://raw.githubusercontent.com/sbluhm/x265/master/x265.spec
rpmbuild -ba x265.spec				// done
wget http://li.nux.ro/download/nux/dextop/el7/SRPMS/fdk-aac-0.1.4-1.src.rpm
rpmbuild --rebuild fdk-aac*			// done
yum install -y ~/rpmbuild/RPMS/armv7hl/faac-devel*  ~/rpmbuild/RPMS/armv7hl/fdk-aac-devel* libcdio-paranoia-devel \
libass-devel libdc1394-devel libmodplug-devel libvdpau-devel openal-soft-devel openjpeg-devel  schroedinger-devel subversion \
soxr-devel texinfo x264-devel ~/rpmbuild/RPMS/armv7hl/x265-devel ~/rpmbuild/RPMS/armv7hl/xvidcore-devel* \
~/rpmbuild/RPMS/armv7hl/xvidcore-1* ~/rpmbuild/RPMS/armv7hl/fdk-aac-0* ~/rpmbuild/RPMS/armv7hl/faac-1* --nogpgcheck
rpmbuild --rebuild ffmpeg* --without=x264	// done
wget http://li.nux.ro/download/nux/dextop/el7/SRPMS/x264-0.142-11.20141221git6a301b6.el7.nux.src.rpm
yum install ~/rpmbuild/RPMS/armv7hl/ffmpeg-devel* ~/rpmbuild/RPMS/armv7hl/ffmpeg-2* ~/rpmbuild/RPMS/armv7hl/ffmpeg-libs* \
~/rpmbuild/RPMS/armv7hl/libavdevice* ~/rpmbuild/RPMS/armv7hl/gpac-0* ~/rpmbuild/RPMS/armv7hl/gpac-devel*
rpmbuild --rebuild x264-0.142-11.20141221git6a301b6.el7.nux.src.rpm
yum install -y ~/rpmbuild/RPMS/armv7hl/x264-devel* ~/rpmbuild/RPMS/armv7hl/x264-0* ~/rpmbuild/RPMS/armv7hl/x264-libs*
rpmbuild --rebuild ffmpeg*			// done
yum reinstall ~/rpmbuild/RPMS/armv7hl/ffmpeg-devel* ~/rpmbuild/RPMS/armv7hl/ffmpeg-2*



wget http://li.nux.ro/download/nux/dextop/el7/SRPMS/libsidplay-1.36.60-2.el7.nux.src.rpm
rpmbuild --rebuild libsidplay-1.36.60-2.el7.nux.src.rpm	// done
wget http://li.nux.ro/download/nux/dextop/el7/SRPMS/a52dec-0.7.4-18.el7.nux.src.rpm
rpmbuild --rebuild a52dec-0.7.4-18.el7.nux.src.rpm	// done
wget http://li.nux.ro/download/nux/dextop/el7/SRPMS/libmad-0.15.1b-16.el7.nux.src.rpm
rpmbuild --rebuild libmad-0.15.1b-16.el7.nux.src.rpm	// done
wget http://li.nux.ro/download/nux/dextop/el7/SRPMS/twolame-0.3.13-3.el7.nux.src.rpm
rpmbuild --rebuild twolame-0.3.13-3.el7.nux.src.rpm	// done
wget http://li.nux.ro/download/nux/dextop/el7/SRPMS/opencore-amr-0.1.3-3.el7.nux.src.rpm
rpmbuild --rebuild opencore-amr-0.1.3-3.el7.nux.src.rpm	// done
wget http://li.nux.ro/download/nux/dextop/el7/SRPMS/lame-3.99.5-2.el7.src.rpm
yum -y install ncurses-devel gtk+-devel --nogpgcheck	// done
rpmbuild --rebuild lame-3.99.5-2.el7.src.rpm		// done
wget http://li.nux.ro/download/nux/dextop/el7/SRPMS/libmpeg2-0.5.1-10.el7.nux.src.rpm
yum -y install SDL-devel				// done
rpmbuild --rebuild libmpeg2-0.5.1-10.el7.nux.src.rpm	// done
yum install libsidplay-1* a52dec-0* lame-3* lame-libs* libmad-0* twolame-0* twolame-libs* opencore-amr-0* \
libsidplay-devel-*  a52dec-devel-* lame-devel-*  libmad-devel-* mpeg2dec-devel-*  twolame-devel-* x264-devel-*  \
opencore-amr-devel-* ~/rpmbuild/RPMS/armv7hl/libmpeg2-devel-0.5.1-10.el7.armv7hl.rpm ~/rpmbuild/RPMS/armv7hl/libmpeg2-0* \
libcdio-devel libid3tag-devel

rpmbuild --rebuild gstreamer1-plugins-ugly-1*
rpmbuild --rebuild gstreamer-plugins-ugly-0*








Install pre-requisite packages:

    # yum -y install gstreamer1 gstreamer1-devel gstreamer1-plugins-ugly gstreamer1-plugins-bad-free gstreamer1-plugins-base gstreamer1-plugins-good libupnp-devel

Install pre-requisite packages (the shortcut):

Install build tools ::

    # yum -y install autoconf automake gcc rpmdevtools git

(Optionally, create less priviledged user for building the rpms' as you should not use root) ::

    # useradd makerpm
    # passwd makerpm
    # su - makerpm

Build the packages ::

    $ rpmdev-setuptree
    $ cd rpmbuild/SOURCES
    $ git clone https://github.com/hzeller/gmrender-resurrect.git
    $ mv gmrender-resurrect gmediarender-0.0.7
    $ tar cjvf gmediarender-0.0.7.tar.bz2 gmediarender-0.0.7
    $ rpmbuild -ba gmediarender-0.0.7/dist-scripts/centos7/gmediarender.spec

After the packages are built, they're be ready at ~/rpmbuild/RPMS/x86_64/ for installation. Source rpm will be ready at ~/rpmbuild/SRPMS/.

Installation can be done easily by ::

    # yum -y install gmediarender-0.*

Note: I would avoid installing them by rpm -i or yum localinstall, as first alter db outside of yum, and latter is deprecated.

Additional configuration is also recommended, sice there's no configuration file for details, this can be achieved by modifing how systemd will start the service. If you want to modify the daemon to listen only on specific IP address and present itself with custom string, follow the steps ::

    # mkdir /etc/systemd/system/gmediarender.service.d
    # vi /etc/systemd/system/gmediarender.service.d/customize.conf   # or nano, or emacs, or whatever editor you like
    [Service]
    ExecStart=
    ExecStart=/usr/bin/gmediarender --port=49494 --ip-address=<your_IP_address> -f "DLNA Renderer GMediaRender"

    # systemctl daemon-reload
    # systemctl start gmediarender.service

If you are using FirewallD, you will also need to open firewall ports for GMediaRender. Custom service files are shipped with the package, just refresh FirewallD and add services to running configuration (and permanent as well, if desired) ::

    # firewall-cmd --reload
    # firewall-cmd --add-service=ssdp
    # firewall-cmd --add-service=gmediarender
