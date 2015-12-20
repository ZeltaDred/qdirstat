# QDirStat
Qt-based directory statistics: KDirStat without any KDE -- from the original KDirStat author.

(c) 2015 Stefan Hundhammer <Stefan.Hundhammer@gmx.de>

License: GPL V2

Updated: 2015-12-19


## Overview

This is a Qt-only port of the old Qt3/KDE3-based KDirStat, now based on the
latest Qt 5.


## Screenshot

![Screenshot of first preview 2015-12-20:]
(https://github.com/shundhammer/qdirstat/blob/master/screenshots/QDirStat-2015-12-20-001.png)
_First preview 2015-12-20 -- see also section "Development Status" below_



## Motivation / Rant

Now that KDE has been losing direction more and more, it's time to come up with
a desktop agnostic version of KDirStat - one that can be used without the
hassle of an ever-changing desktop environment like KDE. KDE has become a
moving target for developers. Much effort has gone into creating eminently
essential things like a yellow note desktop widget (a.k.a. _Plasmoid_) that can
be rotated by 7.834°, but basic usability has gone downhill during that
process.

With the latest Plasma 5 (as of late 2015), there are some KDE apps that use
KDE 5.x, and some that still use KDE 4.x, with the overall effect that settings
have been duplicated (systemsettings5 vs. systemsettings, anyone?), thus the
look and feel between those apps is different, and settings became unaccessible
for the end user (again: systemsettings5 vs. systemsettings).

Worse, users get treated to a mail client that uses a MySQL database back-end
called _Akonadi_. Not only is that the most fragile construction I've ever
seen so far, it also leaves half a Gigabyte worth of log files in my home
directory (!!) due to poor default configuration (both on Kubuntu and on SUSE,
probably also on all other distros). Argh. I found that the most important tool
for that stuff was the _akonadiconsole_ which I needed numerous times to fix a
borked mail environment. But I am a system developer. How on earth is a normal
user supposed to fix that kind of problem? A problem that wouldn't exist in the
first place if the developers hadn't choosen the most complicated and most
fragile combination of tools?

What was once a great desktop environment has become a playground for
developers who don't seem to talk to each other - and, worse, who don't give a
shit about their users anymore.

Time for KDirStat to move away from this insanity. Time to become
independent. KDirStat had never used that much of the KDE infrastructure to
begin with; what little it used (icons, for example) can be replaced by very
little own infrastructure. The KDE libs are just not worth the hassle anymore.


### But what about K4DirStat?

Well, yes, it does exist. I don't want to discount the work that went into that port,
but when I looked at it just a couple of days ago, it was still using the old Qt3
compatibility classes which were deprecated a long time ago. It's only a matter of time
until they will finally be dropped. And the old KDirStat took the old Qt3 QListView / 
QListViewItem very much to its limits, and it used the old Qt3 QCanvas heavily for the
treemaps. So, K4DirStat was a port to Qt4 / KDE4 -- kind of. It does use Qt4 / KDE4 
libs -- technically speaking, but not by spirit. It does not use any of the new 
technologies that Qt4 brought along, neither the model/view based item views nor
QGraphicsView and related.

That's what I did with this recent port: It's now cleanly based on the model/view concept. 
There is no Qt3 compatibility stuff left over. And while I was at it, I also threw out
the other KDE stuff; I didn't really need or want it.


### New Stuff

- Icons are now compiled into the source thanks to Qt's resource system; now it's 
just one binary file, and nothing will go missing.

- The build system is now Qt's _QMake_. I got rid of that _AutoTools_ (Automake, 
Autoconf, Libtool) stuff that most developers find intimidating with its crude M4 macro
processor syntax. QMake .pro files are so much simpler, and they do the job just as well.

- QDirStat now has its own log file. Right now it still logs to /tmp/qdirstat.log, but this
will probably change in the future. No more messages on stdout that either clobber the shell
you started the program from or that simply go missing.


## Current Development Status

- 2015-12-20  First usable preview version - see screenshot below. It's still pretty 
              rough, and sorting via the QSortProxyModel seems to be awfully slow
              once a number of tree branches were ever opened. Looks like I'll have
              to do that myself, too.

- 2015-12-18  Found the crippling bugs that made the DirTreeModel do crazy things.
              None of the Qt classes proved to be helpful to find that -
              they just happily do the crazy things. That's what I call poor design.
              Now there is a first working QDirStat with a useful display tree,
              including icons (but no percentage bar graph yet).

- 2015-12-07  First working DirTreeModel -- still minimalistic, but working.

- 2015-12-06  Created tree model based on QAbstractItemModel.
              Compiles, but dumps core so far.

- 2015-12-05  Imported and ported directory tree data classes.
              Hammered through the compiler, but nothing usable so far.

- 2015-11-28  Project is being set up. Nothing usable so far.



## Building

### Build Environment

Make sure you have a working Qt 5 build environment installed. This includes:

- C++ compiler (gcc recommended)
- Qt 5 runtime environment
- Qt 5 header files


#### Ubuntu

Install the required packages for building:

    sudo apt-get install build-essential qtbase5-dev zlib1g-dev

Dependent packages will be added automatically.

Recommended packages for developers:

    sudo apt-get install qttools5-dev-tools qtbase5-doc qtbase5-doc-html qtbase5-examples

See also

http://askubuntu.com/questions/508503/whats-the-development-package-for-qt5-in-14-04

If you also have a Qt4 development environment installed, select the desired
one via _qtchooser_:

    sudo apt-get install qtchooser
    export QT_SELECT="qt5"


#### SUSE

Install the required packages for building:

    sudo zypper install -t pattern devel_C_C++
    sudo zypper install libQt5Widgets-devel libqt5-qttools zlib-devel

If you also have a Qt4 development environment installed, make sure that the
Qt5 version of 'qmake' is the first in your $PATH:

    export PATH=/usr/lib64/qt5/bin:$PATH


### Compiling

Open a shell window, go to the QDirStat source directory, then enter these
commands:

    qmake
    make


### Installing

    sudo make install

or

    su -c make install


## Contributing

The project is still in its early stages, but contributions are welcome.


### Contribution Guide Lines

Use the same style as the existing code. Look at the old KDE 3 kdirstat for
examples.

Indentation is 4 blanks. One tab is 8 blanks. Do not indent with single tabs
for each indentation level and thus fuck up everybody else's editor. Letting
the editor automatically replace 8 blanks with a tab is okay, but indentation
level is still 4 blanks.

Brace style is braces **always** on a new line. Use blanks after opening and
before closing parentheses:

    if ( someCondition )
    {
        doSomething( arg1, arg2 )
    }

**No** K&R style indentation:

    if (someCondition) {         // WRONG!!!
        doSomething(arg1, arg2)  // WRONG!!!
    }

Use blank lines liberally. No Rubocop-style code.

Use CamelCase for classes and camelCase for variables. Do not use
snake_case. Prefix member variables with an underscore: _someValue. Prefix its
setter with Set...(), use only the name (someValue()) for the getter, **Not**
getSomeValue():

    private:
        SomeType _someValue;    // member variable

    public:
        const & SomeType someValue() const;              // getter
        void setSomeValue( const SomeType & newValue );  // setter

Use a const reference for the setter parameter and the getter return type for
nontrival data types (everything beyond a pointer or an int etc.), and the type
directly otherwise. Use your common sense.

Use Qt types whereever possible. Do not introduce types that are also available
in a Qt version. In particular, do not use STL or Boost types unless there is
no other reasonable way.

For Qt dialogs, use Qt Designer forms whenever possible. I know that many Qt
developers don't like Qt Designer (typically based on bad experiences years
back), but it's really worthwhile to use it. This avoids having tons of dumb
boilerplate code just for creating widget trees.


### Documenting Classes and their Methods

Document classes and their methods in their header file. Document them
immediately. If you don't do it right away, it will never happen. Don't wait
for somebody else to do that for you - nobody will be willing to do the
inconvenient part of the job for you.

Undocumented stuff will mercilessly be rejected.

In my 30+ years of programming I came to the conclusion that it helps immensely
to write down a function name in the header file and **immediately** write its
documentation:

- What does it do? (Do not just repeat the function name! **Explain** what it does!)
- What are the parameters?
- What does it return?
- In particular for functions returning bool: Which case returns 'true', which
  case returns 'false'?
- What happens in fringe cases? (returning NULL pointer?)
- If the function creates any objects: Who owns them?
- Does the function transfer ownership of any objects it gets pointers to as
  parameters to itself or wherever?

If you do that right, you might as well leave the implementation to somebody else.


**Do not** insert any doc template without content before each function.
This is worse than no documentation at all: Everybody has to wade through tons
of useless empty forms that don't contain any content whatsoever.

Be careful when copying and pasting documentation from some other place.
Misleading documentation is worse than no documentation at all.


### Legalese in the Sources

**Do not** copy 30+ lines of legalese bullshit into any source file. One line
in the header like

    License: GPL V2 - see file LICENSE

is plenty. Seriously, what are those people thinking who put all that legalese
into source files? Sure, they listened to spineless corporate lawyers who just
want to make sure. But those lawyers are not going to have that crap smack up
their faces every time they open a file for editing. We developers do.

You lawyers out there, can you here me? **This crap is in the way! Nobody wants
to read that!** It's in the way of people trying to do their jobs! We could
construct a harassment case from this!



### Getting Help for Contributors

- Use the Qt reference documentation.
- Install and study carefully the available Qt examples for reference.
- Use Stack Overflow.
- Use Google.
- Make sure you can build a basic Qt program.
- Make sure you have the compiler and other developer tools set up correctly.
- Make sure you have the relevant -devel (SUSE) or -dev (Ubuntu/Debian)
  packages installed.

If you did all of the above (and only then!) and still couldn't figure out
what's wrong:

- Use IRC (#qdirstat on irc.freenode.net; fallback: contact HuHa in #yast on
  freenode IRC)
- Write a mail

It is a matter of professional courtesy to first use what is freely available
on the web before you consume somebody else's time and expertise. It is utterly
rude to let somebody else point you to the exact part of the Qt documentation
you couldn't be bothered to read.

Does this sound unfriendly? Well, maybe - but: Been there, done that, got the
fucking T-shirt -- countless times. This is not how I want to spend my spare
time.

There are some people who keep arguing that "it takes hours for me to read
through all the documentation, whereas you as an expert can give me the right
answer within a minute".

Well, yes, it might only take a minute to write the answer in IRC, but then
that newbie won't learn anything except that it pays off to disturb people
rather than learn for himself. And reading the documentation is always a good
investment of time; it is never wasted. In the process of searching for an
answer you will come across many things you hadn't known - and that might just
be useful for your next question.

Plus, it will take the expert that one or two minutes to write the answer to
IRC - and then **15-20 minutes to recover from the interrupt**, to restore the
fragile buildings in his mind he needs to solve the problem he was originally
working on.

Please keep in mind that every such interrupt will cost 20-30 minutes of time
on average, and a working day only has 16-24 such time slots; and those experts
tend to be in high demand on many different communication channels (IRC,
personal mail, mailing lists, bugzilla, phone, people walking into the office).

So, should you join the project as a newbie?

If you have patience, self-discipline and are willing to learn (which includes
reading the Qt documentation first), and, most importantly, if you are willing
to stay with the project and not let some unfortunate guy clean up half-ready
stuff, yes.

Just keep in mind that others are doing this because (a) it's fun for them
and/or (b) because they want that piece of software to be successful. Educating
newbies and cleaning up after them is very low on most developers' list of fun
things to do. They still do it, but it's painful for them. Please help
minimizing that pain.

## Reference

Original KDirStat:

Sources: https://github.com/shundhammer/kdirstat

Home page: http://kdirstat.sourceforge.net/
