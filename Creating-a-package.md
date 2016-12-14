# Creating a package installed from source

0. Create an empty file in `/usr/local/lib/crew/packages` with a `.rb` extension. If you wan't to add a recipe for a package called patch, name the file `patch.rb`.

1. Update the content of the file to conform with the following template:
```ruby
require 'package'

class Patch < Package # the name of the package
  version '2.7' # the current version of the package
  source_url 'http://ftp.gnu.org/gnu/patch/patch-2.7.tar.gz' # the source files for the package
  source_sha1 '8886fe94a4cefaf42678ebeca25f4c012bd0f5dc'

  def self.build
    system './configure --prefix=/usr/local' # the steps required to build the package
    system "make"
  end

  def self.install
    system "make", "DESTDIR=#{CREW_DEST_DIR}", "install" # the steps required to install the package
  end
end
```

2. Try your recipe via `crew install *name of package*`, e.g. `crew install patch`.

3. Ensure your package was successfully installed.

4. Create a pull request.

# Creating a binary package
In this tutorial we will compile a package for chromebrew, we will be using [Make](https://www.gnu.org/software/make/) as an example.

0. Make a `src` directory and enter it

        $ mkdir ~/src && cd ~/src

1. Download the source package and extract it

        $ wget ftp://ftp.gnu.org/gnu/make/make-4.1.tar.bz2 && tar -xf make-4.1.tar.bz2

2. **Always** read the `README` and the `INSTALL`

        $ cd make-4.1
        $ nano README
        $ nano INSTALL

3. Install the build dependencies

        $ sudo crew install buildessential

4. Run configure with `--prefix=/usr/local` and any other flags you deem necessary

        $ ./configure --prefix=/usr/local 

5. Compile it, use the `-j` flag to let make use more threads. If compilation fails try again without the `-j` flags.

        $ make -j4

6. Install it to a directory where you can easily find all the files, /usr/local had a lot of other files too.

        $ make install DESTDIR=~/Downloads/make-4.1

7. Go to DESTDIR and run [create_package.sh](https://raw.githubusercontent.com/skycocker/chromebrew/master/create_package.sh)

        $ cd ~/Downloads/make-4.1-chromebrew && wget https://raw.githubusercontent.com/Kriskras99/chromebrew/5ff7b09430390937bc49ffb32a2c1a5b6563113f/create_package.sh
        $ sh create_package.sh

8. There will be a .tar.gz with the name of the DESTDIR directory, upload it to dropbox or any other file sharing site that supports the use of `wget`
9. Then you need to create the packagename.rb, replace packagename with the name of your package.
    ```ruby
    require 'package'

    class Make < Package # the package name
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