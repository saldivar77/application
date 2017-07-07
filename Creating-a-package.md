## Creating a package installed from source

1. Create an empty file in `/usr/local/lib/crew/packages` with a `.rb` extension. If you want to add a recipe for a package called patch, name the file `patch.rb`.

2. Update the content of the file to conform with the following template:
```ruby
require 'package'

class Patch < Package # the name of the package
  description 'Patch takes a patch file containing a difference listing produced by the diff program and applies those differences to one or more original files, producing patched versions.'
  homepage 'http://savannah.gnu.org/projects/patch/'
  version '2.7.5'
  source_url 'https://ftp.gnu.org/gnu/patch/patch-2.7.5.tar.xz'
  source_sha256 'fd95153655d6b95567e623843a0e77b81612d502ecf78a489a4aed7867caa299'

  def self.build
    system './configure --prefix=/usr/local' # the steps required to build the package
    system "make"
  end

  def self.install
    system "make", "DESTDIR=#{CREW_DEST_DIR}", "install" # the steps required to install the package
  end

  def self.check
    system "make", "check" # the steps required to check the package
  end
end
```

3. Try your recipe via `crew install *name of package*`, e.g. `crew install patch`.

4. Ensure your package was successfully installed.

5. Create a pull request.

## Creating a binary package
In this tutorial we will compile a package for chromebrew, we will be using [Make](https://www.gnu.org/software/make/) as an example.

1. Make a `src` directory and enter it

        $ mkdir ~/src && cd ~/src

2. Download the source package and extract it

        $ wget ftp://ftp.gnu.org/gnu/make/make-4.1.tar.bz2 && tar -xf make-4.1.tar.bz2

3. **Always** read the `README` and the `INSTALL`

        $ cd make-4.1
        $ nano README
        $ nano INSTALL

4. Install the build dependencies

        $ sudo crew install buildessential

5. Run configure with `--prefix=/usr/local` and any other flags you deem necessary

        $ ./configure --prefix=/usr/local 

6. Compile it, use the `-j` flag to let make use more threads. If compilation fails try again without the `-j` flags.

        $ make -j4

7. Install it to a directory where you can easily find all the files, /usr/local had a lot of other files too.

        $ make install DESTDIR=~/Downloads/make-4.1

8. Go to DESTDIR and run [create_package.sh](https://raw.githubusercontent.com/skycocker/chromebrew/master/create_package.sh)

        $ cd ~/Downloads/make-4.1-chromebrew && wget https://raw.githubusercontent.com/Kriskras99/chromebrew/5ff7b09430390937bc49ffb32a2c1a5b6563113f/create_package.sh
        $ sh create_package.sh

9. There will be a .tar.gz with the name of the DESTDIR directory, upload it to dropbox or any other file sharing site that supports the use of `wget`
10. Then you need to create the packagename.rb, replace packagename with the name of your package.
    ```ruby
    require 'package'

    class Make < Package # the package name
      description 'GNU Make is a tool which controls the generation of executables and other non-source files of a program from the program\'s source files.'
      homepage 'https://www.gnu.org/software/make/'
      version '4.1' # the package version

      binary_url ({
        i686: 'https://dl.dropboxusercontent.com/s/...', # link to your uploaded package for 32-bit
        x86_64: 'https://dl.dropboxusercontent.com/s/...' # link to your uploaded package for 64-bit
      })
      binary_sha1 ({
        i686: 'a7edc9bdaf9fc72112fe6b370f158a9a1aee87ac', # the sha1sum for your 32-bit package, created by create_package.sh
        x86_64: '1c13b8f261e419a66b87f09653f3fbaf8449efe1' the sha1sum for your 64-bit package, created by create_package.sh
      })

      source_url 'ftp://ftp.gnu.org/gnu/make/make-4.1.tar.bz2' # software source tarball url  
      source_sha1 '7dd39f21bbb1ab176158e0292fd61c47ef681f6d' # software source sha1sum

      depends_on 'buildessential' # add build dependencies here
      depends_on 'gcc'  

      def self.build                                                  # self.build contains commands needed to build the software from source
        system "./configure --prefix=/usr/local"
        system "make"                                                 # ordered chronologically, do not include the -j flag
      end
  
      def self.install                                                # self.install contains commands needed to install the software on the target system
        system "make", "DESTDIR=#{CREW_DEST_DIR}", "install"          # remember to include DESTDIR set to CREW_DEST_DIR - needed to keep track of changes made to system
      end         
    end
```
11. Now create a pull request for your packagename.rb and you are done.