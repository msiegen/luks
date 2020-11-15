# Pure Go library for LUKS volume management

`luks.go` is a pure-Go library that helps to deal with LUKS-encrypted volumes.

Currently, this library is focusing on the read-only path i.e. unlocking a partition without doing
any modifications to LUKS metadata header.

Here is an example that demonstrates the API usage:
```go
dev, err := luks.Open("/dev/sda1");
if err != nil {
  // handle error
}
defer dev.Close()

// equivalent of `cryptsetup open /dev/sda1 volumename`
err = dev.Unlock(/* slot */ 0, []byte("password"), "volumename")
if err == luks.ErrPassphraseDoesNotMatch {
    log.Printf("The password is incorrect")
} else if err != nil {
    log.Print(err)
} else {
    // at this point system should have a file `/dev/mapper/volumename`.
}

// equivalent of `cryptsetup close volumename`
if err := luks.Lock("volumename"); err != nil {
    log.Print(err)
}
```

## License

See [LICENSE](LICENSE).