<center>

## server-manager

[![](https://img.shields.io/crates/v/server-manager.svg)](https://crates.io/crates/server-manager)
[![](https://img.shields.io/crates/d/server-manager.svg)](https://img.shields.io/crates/d/server-manager.svg)
[![](https://docs.rs/server-manager/badge.svg)](https://docs.rs/server-manager)
[![](https://github.com/eastspire/server-manager/workflows/Rust/badge.svg)](https://github.com/eastspire/server-manager/actions?query=workflow:Rust)
[![](https://img.shields.io/crates/l/server-manager.svg)](./LICENSE)

</center>

[Official Documentation](https://docs.ltpp.vip/server-manager/)

[Api Docs](https://docs.rs/server-manager/latest/server_manager/)

> server-manager is a rust library for managing server processes. It encapsulates service startup, shutdown, and background daemon mode. Users can specify the PID file, log file paths, and other configurations through custom settings, while also passing in their own asynchronous server function for execution. The library supports both synchronous and asynchronous operations. On Unix and Windows platforms, it enables background daemon processes.

## Installation

To use this crate, you can run cmd:

```shell
cargo add server-manager
```

## Use

```rust
use server_manager::*;
use std::fs;
use std::time::Duration;

let pid_file: String = "./process/test_pid.pid".to_string();
let _ = fs::remove_file(&pid_file);
let config: ServerManagerConfig = ServerManagerConfig {
    pid_file: pid_file.clone(),
};
let dummy_server = || async {
    tokio::time::sleep(Duration::from_secs(1)).await;
};
let manager = ServerManager::new(config, dummy_server);
let res: ServerManagerResult = manager.start_daemon();
println!("start_daemon {:?}", res);
let res: ServerManagerResult = manager.stop();
println!("stop {:?}", res);
manager.start().await;
let _ = fs::remove_file(&pid_file);
let res: ServerManagerResult =
    manager.hot_restart(&["--once", "-x", "check", "-x", "build --release"]);
println!("hot_restart {:?}", res);
```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.

## Contact

For any inquiries, please reach out to the author at [root@ltpp.vip](mailto:root@ltpp.vip).
