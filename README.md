# Local build with neon docker

Make sure docker is installed for you distro and you have access to it (you are in docker group)

```
mkdir /tmp/konvi-build # make build dir we can mount into docker
docker pull kdeneon/plasma:dev-unstable # pull neon image
docker run -it -v /tmp/konvi-build:/workspace kdeneon/plasma:dev-unstable bash
```

You now have a docker container with `/tmp/konvi-build/` mounted into `/workspace/`.
In the container we can do the build

```
git clone https://github.com/apachelogger/konversation-qtquick-snap.git
apt update
apt install snapcraft
snapcraft --debug
```

On the host you can now install the snap.

```
sudo snap install --force-dangerous --devmode /tmp/konvi-build/*.snap
```

# Jenkins build

Hit a build here https://build.neon.kde.org/view/testy/job/test_konversation-qtquick/lastBuild/artifact/

Gets published to store (unless there's a problem with auto-validation)

```
sudo snap install --devmode --channel edge konversation-qtquick
```
