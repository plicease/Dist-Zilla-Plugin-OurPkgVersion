# NAME

Dist::Zilla::Plugin::OurPkgVersion - No line insertion and does Package version with our

# VERSION

version 0.12

# SYNOPSIS

in dist.ini

        [OurPkgVersion]

in your modules

        # VERSION

# DESCRIPTION

This module was created as an alternative to
[Dist::Zilla::Plugin::PkgVersion](https://metacpan.org/pod/Dist::Zilla::Plugin::PkgVersion) and uses some code from that module. This
module is designed to use a the more readable format `our $VERSION =
$version;` as well as not change then number of lines of code in your files,
which will keep your repository more in sync with your CPAN release. It also
allows you slightly more freedom in how you specify your version.

## EXAMPLES

in dist.ini

        ...
        version = 0.01;
        [OurPkgVersion]

in lib/My/Module.pm

        package My::Module;
        # VERSION
        ...

output lib/My/Module.pm

        package My::Module;
        our $VERSION = '0.01'; # VERSION
        ...

please note that whitespace before the comment is significant so

        package My::Module;
        BEGIN {
                # VERSION
        }
        ...

becomes

        package My::Module;
        BEGIN {
                our $VERSION = '0.01'; # VERSION
        }
        ...

while

        package My::Module;
        BEGIN {
        # VERSION
        }
        ...

becomes

        package My::Module;
        BEGIN {
        our $VERSION = '0.01'; # VERSION
        }
        ...

you can also add additional comments to your comment

        ...
        # VERSION: generated by DZP::OurPkgVersion
        ...

becomes

        ...
        our $VERSION = '0.1.0'; # VERSION: generated by DZP::OurPkgVersion
        ...

you can also use perltidy's default static side comments (##)

        ...
        ## VERSION
        ...

becomes

        ...
        our $VERSION = '0.1.0'; ## VERSION
        ...

Also note, the package line is not in any way significant, it will insert the
`our $VERSION` line anywhere in the file before `# VERSION` as many times as
you've written `# VERSION` regardless of whether or not inserting it there is
a good idea. OurPkgVersion will not insert a version unless you have `#
VERSION` so it is a bit more work.

If you make a trial release, the comment will be altered to say so:

        # VERSION

becomes

        our $VERSION = '0.01'; # TRIAL VERSION

# METHODS

- munge\_files

    Override the default provided by [Dist::Zilla::Role::FileMunger](https://metacpan.org/pod/Dist::Zilla::Role::FileMunger) to limit
    the number of files to search to only be modules and executables.

- munge\_file

    tells which files to munge, see [Dist::Zilla::Role::FileMunger](https://metacpan.org/pod/Dist::Zilla::Role::FileMunger)

- finder

    Override the default [FileFinder](https://metacpan.org/pod/Dist::Zilla::Role::FileFinder) for
    finding files to check and update. The default value is `:InstallModules`
    and `:PerlExecFiles` (when available; otherwise `:ExecFiles`)
    \-- see also [Dist::Zilla::Plugin::ExecDir](https://metacpan.org/pod/Dist::Zilla::Plugin::ExecDir), to make sure the script
    files are properly marked as executables for the installer.

# PROPERTIES

- underscore\_eval\_version

    For version numbers that have an underscore in them, add this line
    immediately after the our version assignment:

        $VERSION = eval $VERSION;

    This is arguably the correct thing to do, but changes the line numbering
    of the generated Perl module or source, and thus optional.

- skip\_main\_module

    Set to true to ignore the main module in the distribution. This prevents
    a warning when using [Dist::Zilla::Plugin::VersionFromMainModule](https://metacpan.org/pod/Dist::Zilla::Plugin::VersionFromMainModule) to
    obtain the version number instead of the `dist.ini` file.

# BUGS

Please report any bugs or feature requests on the bugtracker website
[https://github.com/plicease/dist-zilla-plugin-ourpkgversion/issues](https://github.com/plicease/dist-zilla-plugin-ourpkgversion/issues)

When submitting a bug or request, please include a test-file or a
patch to an existing test-file that illustrates the bug or desired
feature.

# CONTRIBUTORS

- Alexandr Ciornii <alexchorny@gmail.com>
- Alexei Znamensky <russoz@cpan.org>
- Christian Walde <walde.christian@googlemail.com>
- Christopher J. Madsen <perl@cjmweb.net>
- David Golden <dagolden@cpan.org>
- Graham Ollis <perl@wdlabs.com>
- Graham Ollis <plicease@cpan.org>
- Ian Sealy <git@iansealy.com>
- Michael Jemmeson <mjemmeson@cpan.org>
- Stephan Loyd <stephanloyd9@gmail.com>

# AUTHORS

- Caleb Cushing <xenoterracide@gmail.com>
- Grahan Ollis <plicease@cpan.org>

# COPYRIGHT AND LICENSE

This software is Copyright (c) 2019 by Caleb Cushing.

This is free software, licensed under:

    The Artistic License 2.0 (GPL Compatible)
