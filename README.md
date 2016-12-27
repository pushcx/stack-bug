`stack`'s `ghci` command breaks importing from the current directory.

    ~/code/stack-bug $ ghci
    GHCi, version 7.10.3: http://www.haskell.org/ghc/  :? for help
    λ> :l Foo
    [1 of 2] Compiling Bar              ( Bar.hs, interpreted )
    [2 of 2] Compiling Foo              ( Foo.hs, interpreted )
    Ok, modules loaded: Foo, Bar.
    λ> 
    Leaving GHCi.
    ~/code/stack-bug $ stack ghci
    Configuring GHCi with the following packages: 
    GHCi, version 7.10.3: http://www.haskell.org/ghc/  :? for help
    λ> :l Foo

    Foo.hs:3:8:
        Could not find module ‘Bar’
        Perhaps you meant
          Bag (needs flag -package-key ghc-7.10.3)
          Var (needs flag -package-key ghc-7.10.3)
    Failed, modules loaded: none.
    λ> 

---

Thanks to [Christopher Allen](https://twitter.com/bitemyapp) for pointing the way towards a fix.

You can work around the bug by re-including the current directory:

    $ stack ghci --ghci-options -i.

