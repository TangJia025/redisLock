# go redisLock

    golang redis分布式锁
    使用demo:

    import (
        "github.com/daheige/redisLock"
        "log"
    )

    func main(){
        conn, err := redis.Dial("tcp", "localhost:6379")
        if err != nil {
            log.Println("redis connection error: ", err)
            return
        }

        defer conn.Close()

        l := redisLock.New(conn, "heige", "hello,world", 100)

        if ok, err := l.TryLock(); ok {
            log.Println("lock success")
            for i := 0; i < 10; i++ {
                log.Println("hello,i: ", i)
            }

            l.Unlock()
        } else {
            log.Println("lock fail")
            log.Println("err: ", err)
        }
    }
