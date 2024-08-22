# lsmdb
一个lua使用简单的内存/文件数据库！

# 依赖
- [lua](https://github.com/xiyoo0812/lua.git)5.2以上
- [luakit](https://github.com/xiyoo0812/luakit.git)一个luabind库
- 项目路径如下<br>
  |--proj <br>
  &emsp;|--lua <br>
  &emsp;|--lsmdb <br>
  &emsp;|--luakit

# 编译
- msvc: 准备好lua依赖库并放到指定位置，将proj文件加到sln后编译。
- linux: 准备好lua依赖库并放到指定位置，执行make -f lsmdb.mak

# 用法
```lua
-- smdb_test.lua
require("lsmdb")
local log_debug     = logger.debug
local jsoncodec     = json.jsoncodec

local driver = smdb.create()
local jcodec = jsoncodec()

driver.set_codec(jcodec)
local ok = driver.open("./smdb/xxx.db")
log_debug("open: {}", ok)

local a = driver.put("abc1", {a=123})
local b = driver.put("abc2", "234")
local c = driver.put("abc3", "335")
local d = driver.put("abc4", "436")
local e = driver.put("abc5", "536")

log_debug("put: {}-{}-{}-{}-{}", a, b, c, d, e)

for i = 1, 6 do
    local key = "abc" .. i
    local da = driver.get(key)
    log_debug("get-{}: {}", key, da)
end

local k, v = driver.first()
while v do
    log_debug("cursor: {}={}", k, v)
    k, v = driver.next()
end

b = driver.del("abc2")
a = driver.del("abc5")
c = driver.put("abc3", "4335")
log_debug("del: {}-{}-{}", a, b, c)


for i = 1, 6 do
    local key = "abc" .. i
    local da = driver.get(key)
    log_debug("del_get-{}: {}", key, da)
end

```
