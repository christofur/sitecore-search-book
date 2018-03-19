## Lucene analyzer demonstrating tokenizer and chained filters


`ChainedFiltersAnalyzer.cs`
```csharp
using System.IO;
using Lucene.Net.Analysis;
using Lucene.Net.Analysis.Standard;
using Version = Lucene.Net.Util.Version;

public class ChainedFiltersAnalyzer : Analyzer
{
    private const Version _version = Version.LUCENE_30;

    public override TokenStream TokenStream(string fieldName, TextReader reader)
    {
        //1: Tokenize
        var tokenStream = ((TokenStream) new StandardTokenizer(_version, reader));

        //2: Filter
        var standardFilteredStream = (TokenStream) new StandardFilter(tokenStream);
        var stopFilteredStream = new StopFilter(true, standardFilteredStream, StopAnalyzer.ENGLISH_STOP_WORDS_SET, true);
        var lowercaseFilteredStream = new LowerCaseFilter(stopFilteredStream);
        var porterStemFilteredStream = new PorterStemFilter(lowercaseFilteredStream);
        return porterStemFilteredStream;
    }
}
```
