# appconfig -- application config and command line arg setting library
简单的应用程序命令行设置及配置文件读取库

---
#### 项目地址
<https://github.com/kivensoft/appconfig_rs>

###### 第三方依赖
* thiserror
* rand

---
###### 添加依赖
`cargo add --git https://github.com/kivensoft/appconfig_rs appconfig`
###### 使用
```rust
use appconfig;

appconfig::appconfig_define!(app_conf, AppConf,
    log_level   : String => ["L",  "log-level",    "LogLevel",          "log level(trace/debug/info/warn/error/off)"],
    log_file    : String => ["F",  "log-file",     "LogFile",           "log filename"],
    log_max     : String => ["M",  "log-max",      "LogFileMaxSize",    "log file max size (unit: k/m/g)"],
    log_async   : bool   => ["",   "log-async",    "LogAsync",          "enable asynchronous logging"],
    no_console  : bool   => ["",   "no-console",   "NoConsole",         "prohibit outputting logs to the console"],
);

impl Default for AppConf {
    fn default() -> Self {
        Self {
            log_level   : String::from("info"),
            log_file    : String::with_capacity(0),
            log_max     : String::from("10m"),
            log_async   : false,
            no_console  : false,
        }
    }
}

fn main() {
    let ac = AppConf::init();
    if !appconfig::parse_args(ac, version).expect("parse args error") {
        return;
    }

	println!("arg log_file = {}", ac.log_file);
}
```
