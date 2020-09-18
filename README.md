# powerbi_snippets
Contains code snippets and information related to PowerBI.

Snippet #1:
Integrating PowerBI with REST API backend servers to retrieve json or csv formatted data.

There is the query editor and the advanced query editor to give full control over the way to fetch data.

![alt text](https://github.com/bjhulst/powerbi_snippets/blob/master/doc/powerbi_advanced_query_editor.png?raw=true)

The code below is fetching data from a REST API endpoint after OAUTH authentication has provided a access_token.

```let
    MyToken = let
        GetJson = Web.Contents("https://api.acme.com/api/oauth",
        [Headers = [#"Accept"="application/json",#"Content-Type"="application/json"],
        Content = Text.ToBinary("{""username"": "”demoaccount"" ,""password"":"”HardToGuessPassWordHere"" }")]),
        FormatAsJson = Json.Document(GetJson),
        token = FormatAsJson[access_token],
        access_token=token
    in
        access_token,
    GetCSVData = Web.Contents( "https://api.acme.com/api/reporting/events?y=2019&m=04&d=01",
    [Headers=[Authorization="Bearer "&MyToken,ContentType="application/json",#"Accept"="application/csv"],
    Content = Text.ToBinary("{""endpoint"": "”events""}")]),
    Result = Table.PromoteHeaders(Csv.Document(GetCSVData))
in
    Result
```


References:
  * https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-use-directquery
