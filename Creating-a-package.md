## Creating a package installed from source

1. Create an empty file in `/usr/local/lib/crew/packages` with a `.rb` extension. If you want to add a recipe for a package called bc, name the file `bc.rb`.

2. Update the content of the file to conform with the following template:
```ruby
require 'package'

class Bc < Package                 # The first character of the class name must be upper case
  description 'bc is an arbitrary precision numeric processing language.'
  homepage 'http://www.gnu.org/software/bc/'
  version '1.07.1'
  source_url 'https://ftp.gnu.org/gnu/bc/bc-1.07.1.tar.gz'
  source_sha256 '62adfca89b0a1c0164c2cdca59ca210c1d44c3ffc46daf9931cf4942664cb02a'   # Use the command "sha256sum"

  depends_on 'readline'            # packages required by this package
  depends_on 'flex' => :build      # packages with "=> :build" are only required if you're building from source 
  depends_on 'ed' => :build

  def self.build                   # the steps required to build the package
    system "./configure", "--with-readline", "--prefix=/usr/local"
    system "make"
  end

  def self.install                 # the steps required to install the package
    system "make", "DESTDIR=#{CREW_DEST_DIR}", "install"
  end

  def self.check                   # the steps required to check if the package was built ok
    system "make", "check"
  end
end
```
3. Use "--prefix=#{CREW_PREFIX}" on configure and "DESTDIR=#{CREW_DEST_DIR}" on install to make sure everything ends up in the correct place.

4. Try your recipe via `crew install pkgname`, e.g. `crew install bc`.

5. Ensure your package was successfully installed.

6. Create a pull request.

## Adding a pre-compiled binary

To add a pre-compiled binary file to your new package or to an existing one, you must first create a source-only package like above if it does not exist. After that:

1. Use `crew build pkgname`, e.g. `crew build bc`. 

2. If you see `pkgname-version-chromeos-type.tar.xz is built!`, it means everything worked fine and the compressed file has everything you need. It will also create a .sha256 file that has the sha256 of the .tar.xz file.

3. Upload the .tar.xz file to any file sharing site that supports the use of `wget`. For now, the recommended approach is to create a release in your chromebrew fork and upload it in the release files.

4. Modify the package .rb to add the pre-compiled binaries and their checksums.

```ruby
require 'package'

class Bc < Package
  # (...) everything as before
  depends_on 'readline'
  depends_on 'flex' => :build
  depends_on 'ed' => :build

  binary_url({
    # link to the pre-compiled package
    x86_64: 'https://github.com/your_github_username/chromebrew/releases/download/release_name/bc-1.07.1-chromeos-x86_64.tar.xz'
  })

  binary_sha256({
    # sha256 of the .tar.xz file
    x86_64: '1a5ab1f567e35e73388deb50944306a170132ce948163d077d0ddf84b272368b'
  })

  # (...) more things as before
end
```

5. Make sure to add to the correct architecture. In the example, we used x86_64, but it could also be i686, armv7l or aarch64 (the name will be included in the .tar.xz filename). There may be more than one type of pre-compiled binary; each for a different architecture.

6. Create a pull request to submit the changes.