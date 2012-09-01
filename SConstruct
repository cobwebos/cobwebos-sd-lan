"""The SConstruct file"""

name = 'freelan'
major = '2'
minor = '0'
libraries = []

# You should not need to modify anything below this line

import os, sys

from freelan.buildtools import LibraryProject, Environment

env = Environment(ENV = os.environ.copy(), ARGUMENTS = ARGUMENTS)

if sys.platform.startswith('win32'):
    libraries.append('asiotap_static')
    libraries.append('fscp_static')
    libraries.append('cryptoplus_static')
else:
    libraries.append('asiotap')
    libraries.append('fscp')
    libraries.append('cryptoplus')

libraries.append('boost_system')
libraries.append('boost_thread')

if sys.platform.startswith('win32'):

    if env['CC'] == 'gcc':
        libraries.append('crypto')
        libraries.append('curl')
        env['CXXFLAGS'].append('-DBOOST_THREAD_USE_LIB')
        env['CXXFLAGS'].append('-DBOOST_USE_WINDOWS_H')
        env['CXXFLAGS'].append('-D_WIN32_WINNT=0x0501')
        env['CXXFLAGS'].append('-DCURL_STATICLIB')
    else:
        libraries.append('libeay32')
        libraries.append('advapi32')
        libraries.append('shell32')
        libraries.append('libcurl_a')

        env['CXXFLAGS'].append('/DBOOST_THREAD_USE_LIB')
        env['CXXFLAGS'].append('/DBOOST_USE_WINDOWS_H')
        env['CXXFLAGS'].append('/D_WIN32_WINNT=0x0501')
        env['CXXFLAGS'].append('/DCURL_STATICLIB')

    libraries.append('ws2_32')
    libraries.append('gdi32')
    libraries.append('iphlpapi')
else:
    libraries.append('pthread')
    libraries.append('curl')

    if sys.platform.startswith('linux2'):
        libraries.append('rt')

    if sys.platform.startswith('darwin'):
        libraries.append('crypto')

env.Append(CPPPATH=['src/extras/rapidjson/include'])

project = LibraryProject(name, major, minor, libraries, Glob('src/*.cpp'))

build = env.FreelanProject(project)
install = env.FreelanProjectInstall(project)
documentation = env.FreelanProjectDocumentation(project)
indent = env.FreelanProjectIndent(project)
samples = env.SConscript('samples/SConscript', exports = 'env project')

env.Alias('build', build)
env.Alias('install', install)
env.Alias('doc', documentation)
env.Alias('indent', indent)
env.Alias('samples', samples)

env.Default(build)
