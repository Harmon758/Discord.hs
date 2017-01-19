# Discord.hs
A Haskell wrapper for the Discord API

## Installing
Discord-hs isn't on hackage or stackage (Yet, I'll upload when documentation is in a good state.)  
In order to install:
```sh
git clone https://github.com/jano017/Discord.hs.git
cd discord.hs
cabal install
```
Stack support to come.

## PingPong
```haskell
{-# LANGUAGE OverloadedStrings, RecordWildCards #-}
import Data.Text
import Pipes

import Network.Discord
import Language.Discord

reply :: Message -> Text -> Effect DiscordM ()
reply Message{messageChannel=chan} cont = fetch' $ CreateMessage chan cont

main :: IO ()
main = runBot "TOKEN" $ do
  with ReadyEvent $ \(Init v u _ _ _) ->
    liftIO . putStrLn $ "Connected to gateway v" ++ show v ++ " as user " ++ show u

  with MessageCreateEvent $ \msg@Message{..} -> do
    when ("Ping" `isPrefixOf` messageContent && (not . userIsBot $ messageAuthor)) $
      reply msg "Pong!"
```

## Known issues:
- Init isn't parsing correctly
- Client doesn't close correctly
- All json serialization is currently in Types.Channel. Should be broken out
- Only Channel/Guild api endpoints are implemented
- Missing voice support
- Stack dependencies not locked down

## Future goals:
- [Eta](https://github.com/typelead/eta) compatibility
- [HaLVM](https://github.com/GaloisInc/HaLVM) compatibility (maybe)
- Possibly split into multiple packages?
- Tighten properties/prove properties
- Monad based declarative bots (See Language.Discord. Needs further documentation/integration)
- Async/threading
- Proper logging?
- Reactions
- Embeds
