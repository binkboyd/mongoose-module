## Zephyr Mongoose Module Repository

Repository create by taking advantage of following documents:

- [manifest](https://docs.zephyrproject.org/latest/develop/west/manifest.html#)
- [modules](https://docs.zephyrproject.org/latest/develop/modules.html#)

I suggest to read following zephyr issues:

- [add http server support pr](https://github.com/zephyrproject-rtos/zephyr/pull/64465)
- [replacement for civetweb](https://github.com/zephyrproject-rtos/zephyr/issues/46758)

### Usage

- first one add mongoose module into main manifest(west.yaml) following way:

```yaml
  remotes:
    - name: zephyrproject-rtos
      url-base: https://github.com/zephyrproject-rtos
    - name: cesanta
      url-base: https://github.com/cesanta
    - name: sparse
      url-base: https://github.com/Sparse-Technology/mongoose-module

  projects:
    - name: zephyr
      remote: zephyrproject-rtos
      revision: main
      import:
        # By using name-allowlist we can clone only the modules that are
        # strictly needed by the application.
        name-allowlist:
          - cmsis      # required by the ARM port
          - hal_nordic # required by the custom_plank board (Nordic based)
          - hal_stm32  # required by the nucleo_f302r8 board (STM32 based)
          - mbedtls

    - name: mongoose
      remote: sparse # referred from remotes can change other forks or main
      submodules: true # cesanta mongoose add as submodule
      revision: master # can select different branch
      repo-path: .
      path: modules/lib/mongoose
```

- second one run `west update` and initialized mongoose module

- third one can add your prj.conf for using mongoose library.

```
CONFIG_MONGOOSE=y

CONFIG_NETWORKING=y
CONFIG_NET_IPV4=y
CONFIG_NET_IPV6=y
CONFIG_NET_TCP=y
CONFIG_NET_UDP=y
CONFIG_NET_DHCPV4=y
CONFIG_NET_SOCKETS=y
CONFIG_NET_SOCKETS_POLL_MAX=32
CONFIG_POSIX_MAX_FDS=32
CONFIG_NET_MAX_CONN=10
CONFIG_NET_MAX_CONTEXTS=10
CONFIG_NET_CONFIG_SETTINGS=y
CONFIG_NET_CONNECTION_MANAGER=y
CONFIG_NET_LOG=y

CONFIG_LOG=y
CONFIG_ISR_STACK_SIZE=2048
CONFIG_MAIN_STACK_SIZE=8192
CONFIG_IDLE_STACK_SIZE=1024

CONFIG_MINIMAL_LIBC=y
CONFIG_MINIMAL_LIBC_RAND=y
CONFIG_MINIMAL_LIBC_MALLOC_ARENA_SIZE=32768
```

- add library and ready to start coding:

```c
#include <mongoose.h>
```
