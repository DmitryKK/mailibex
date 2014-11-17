mailibex
========

Library containing Email related implementations in Elixir : dkim, spf, dmark, mimemail, smtp

## MimeMail ##

Use `MimeMail.from_string` to split email headers and body, this function keep
the raw representation of headers and body in a `{:raw,value}` term.

Need more explanations about pluggable header Codec...

`MimeMail.to_string` encode headers and body into the final ascii mail.

## FlatMail ##

Flat mail representation of MimeMail is simply a `KeywordList` where
all the keys `[:txt,:html,:attach,:attach_in,:include]` are used to construct the body tree of 
alternative/mixed/related multiparts in the `body` field of the
`MimeMail` struct, and the rest of the `KeywordList` became the
`header` field.

```elixir
MimeMail.Flat.to_mail(from: "me@example.org", txt: "Hello world", attach: "attached plain text", attach: File.read!("attachedfile"))
|> MimeMail.to_string
```

Need more explanations here...

## DKIM ##

```elixir
[rsaentry] =  :public_key.pem_decode(File.read!("test/mails/key.pem"))
mail
|> MimeMail.from_string
|> DKIM.sign(:public_key.pem_entry_decode(rsaentry), d: "order.brendy.fr", s: "cobrason")
```

Need more explanations here...

## DMARC ##

Organizational Domain implementation using public suffix database : 
(https://publicsuffix.org/list/effective_tld_names.dat)

```elixir
"orga2.gouv.fr" = DMARK.organization "orga0.orga1.orga2.gouv.fr"
```

## Current Status

- DKIM is fully implemented (signature/check), missing DKIM-Quoted-Printable token management
- mimemail encoding/decoding of headers and body are fully implemented
- flat mime body representation for easy mail creation / modification
- DMARC implementation of organizational domains

## TODO :

- SPF check
- DMARC report
- smtp client implementation
- smtp server implementation over Ranch

