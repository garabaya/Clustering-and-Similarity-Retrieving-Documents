# Document retrieval assignament (Semana 4)

Jupyter Notebook for retrieving articles from Wikipedia about famous people focused on using nearest neighbors and clustering by analyzing their text.


```python
import turicreate
```


```python
people = turicreate.SFrame('people_wiki.sframe')
```

## 3 words in Elton John's article with highest word count


```python
people['word_count'] = turicreate.text_analytics.count_words(people['text'])
```


```python
elton = people[people['name'] == 'Elton John']
```


```python
elton
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">URI</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">name</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">text</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">word_count</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">&lt;http://dbpedia.org/resou<br>rce/Elton_John&gt; ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Elton John</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">sir elton hercules john<br>cbe born reginald ken ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">{&#x27;movements&#x27;: 1.0,<br>&#x27;social&#x27;: 1.0, ...</td>
    </tr>
</table>
[? rows x 4 columns]<br/>Note: Only the head of the SFrame is printed. This SFrame is lazily evaluated.<br/>You can use sf.materialize() to force materialization.
</div>




```python
elton.stack('word_count',new_column_name=['word','count'])
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">URI</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">name</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">text</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">word</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">count</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">&lt;http://dbpedia.org/resou<br>rce/Elton_John&gt; ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Elton John</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">sir elton hercules john<br>cbe born reginald ken ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">movements</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">&lt;http://dbpedia.org/resou<br>rce/Elton_John&gt; ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Elton John</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">sir elton hercules john<br>cbe born reginald ken ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">social</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">&lt;http://dbpedia.org/resou<br>rce/Elton_John&gt; ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Elton John</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">sir elton hercules john<br>cbe born reginald ken ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">champion</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">&lt;http://dbpedia.org/resou<br>rce/Elton_John&gt; ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Elton John</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">sir elton hercules john<br>cbe born reginald ken ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">wed</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">&lt;http://dbpedia.org/resou<br>rce/Elton_John&gt; ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Elton John</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">sir elton hercules john<br>cbe born reginald ken ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">legal</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">&lt;http://dbpedia.org/resou<br>rce/Elton_John&gt; ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Elton John</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">sir elton hercules john<br>cbe born reginald ken ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">became</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">&lt;http://dbpedia.org/resou<br>rce/Elton_John&gt; ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Elton John</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">sir elton hercules john<br>cbe born reginald ken ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">after</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">&lt;http://dbpedia.org/resou<br>rce/Elton_John&gt; ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Elton John</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">sir elton hercules john<br>cbe born reginald ken ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2005</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">&lt;http://dbpedia.org/resou<br>rce/Elton_John&gt; ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Elton John</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">sir elton hercules john<br>cbe born reginald ken ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">december</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">&lt;http://dbpedia.org/resou<br>rce/Elton_John&gt; ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Elton John</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">sir elton hercules john<br>cbe born reginald ken ...</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">furnish</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2.0</td>
    </tr>
</table>
[255 rows x 5 columns]<br/>Note: Only the head of the SFrame is printed.<br/>You can use print_rows(num_rows=m, num_columns=n) to print more rows and columns.
</div>




```python
elton_word_count_table = elton[['word_count']].stack('word_count', new_column_name = ['word','count'])
```


```python
elton_word_count_table.sort('count',ascending=False)
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">word</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">count</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">the</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">27.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">in</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">18.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">and</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">15.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">of</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">13.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">a</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">10.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">has</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">9.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">he</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">7.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">john</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">7.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">on</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">6.0</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">since</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5.0</td>
    </tr>
</table>
[255 rows x 2 columns]<br/>Note: Only the head of the SFrame is printed.<br/>You can use print_rows(num_rows=m, num_columns=n) to print more rows and columns.
</div>



## 3 words in Elton John's article with highest TDIDF


```python
people['tfidf'] = turicreate.text_analytics.tf_idf(people['text'])
```


```python
elton = people[people['name'] == 'Elton John']
```


```python
elton_tdidf_table = elton[['tfidf']].stack('tfidf', new_column_name = ['word','tfidf'])
```


```python
elton_tdidf_table.sort('tfidf',ascending=False)
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">word</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">tfidf</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">furnish</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">18.38947183999428</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">elton</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">17.482320270031995</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">billboard</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">17.30368095754203</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">john</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">13.93931279239831</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">songwriters</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">11.250406447031539</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">overallelton</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">10.986495389225194</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">tonightcandle</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">10.986495389225194</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">19702000</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">10.293348208665249</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">fivedecade</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">10.293348208665249</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">aids</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">10.262846934045534</td>
    </tr>
</table>
[255 rows x 2 columns]<br/>Note: Only the head of the SFrame is printed.<br/>You can use print_rows(num_rows=m, num_columns=n) to print more rows and columns.
</div>



## Measuring distance


```python
victoria = people[people['name'] == 'Victoria Beckham']
paul = people[people['name'] == 'Paul McCartney']
```


```python
turicreate.distances.cosine(elton['tfidf'][0],victoria['tfidf'][0])
```




    0.9567006376655429




```python
turicreate.distances.cosine(elton['tfidf'][0],paul['tfidf'][0])
```




    0.8250310029221779



## Building nearest neighbors models with different input features and setting the distance metric


```python
knn_model_tfidf = turicreate.nearest_neighbors.create(people,features=['tfidf'],label='name',distance='cosine')
```


<pre>Starting brute force nearest neighbors model training.</pre>



```python
knn_model_word_count = turicreate.nearest_neighbors.create(people,features=['word_count'],label='name',distance='cosine')
```


<pre>Starting brute force nearest neighbors model training.</pre>



```python
knn_model_word_count.query(elton)
```


<pre>Starting pairwise querying.</pre>



<pre>+--------------+---------+-------------+--------------+</pre>



<pre>| Query points | # Pairs | % Complete. | Elapsed Time |</pre>



<pre>+--------------+---------+-------------+--------------+</pre>



<pre>| 0            | 1       | 0.00169288  | 26.294ms     |</pre>



<pre>| Done         |         | 100         | 505.63ms     |</pre>



<pre>+--------------+---------+-------------+--------------+</pre>





<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">query_label</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">reference_label</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">distance</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">rank</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Elton John</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2.220446049250313e-16</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Cliff Richard</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.16142415258967036</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Sandro Petrone</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.16822542751041114</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Rod Stewart</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.16832716558706107</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">4</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Malachi O&#x27;Doherty</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.177315545978884</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5</td>
    </tr>
</table>
[5 rows x 4 columns]<br/>
</div>




```python
knn_model_tfidf.query(elton)
```


<pre>Starting pairwise querying.</pre>



<pre>+--------------+---------+-------------+--------------+</pre>



<pre>| Query points | # Pairs | % Complete. | Elapsed Time |</pre>



<pre>+--------------+---------+-------------+--------------+</pre>



<pre>| 0            | 1       | 0.00169288  | 21.418ms     |</pre>



<pre>| Done         |         | 100         | 660.856ms    |</pre>



<pre>+--------------+---------+-------------+--------------+</pre>





<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">query_label</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">reference_label</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">distance</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">rank</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Elton John</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">-2.220446049250313e-16</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Rod Stewart</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.7172196678927374</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">George Michael</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.7476009989692848</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Sting (musician)</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.7476719544306141</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">4</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Phil Collins</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.7511932487904706</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5</td>
    </tr>
</table>
[5 rows x 4 columns]<br/>
</div>




```python
knn_model_word_count.query(victoria)
```


<pre>Starting pairwise querying.</pre>



<pre>+--------------+---------+-------------+--------------+</pre>



<pre>| Query points | # Pairs | % Complete. | Elapsed Time |</pre>



<pre>+--------------+---------+-------------+--------------+</pre>



<pre>| 0            | 1       | 0.00169288  | 13.998ms     |</pre>



<pre>| Done         |         | 100         | 476.97ms     |</pre>



<pre>+--------------+---------+-------------+--------------+</pre>





<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">query_label</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">reference_label</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">distance</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">rank</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Victoria Beckham</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">-2.220446049250313e-16</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Mary Fitzgerald (artist)</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.20730703611504997</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Adrienne Corri</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.21450978278754795</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Beverly Jane Fry</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.21746646874079278</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">4</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Raman Mundair</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.21769547499150488</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5</td>
    </tr>
</table>
[5 rows x 4 columns]<br/>
</div>




```python
knn_model_tfidf.query(victoria)
```


<pre>Starting pairwise querying.</pre>



<pre>+--------------+---------+-------------+--------------+</pre>



<pre>| Query points | # Pairs | % Complete. | Elapsed Time |</pre>



<pre>+--------------+---------+-------------+--------------+</pre>



<pre>| 0            | 1       | 0.00169288  | 22.102ms     |</pre>



<pre>| Done         |         | 100         | 683.52ms     |</pre>



<pre>+--------------+---------+-------------+--------------+</pre>





<div style="max-height:1000px;max-width:1500px;overflow:auto;"><table frame="box" rules="cols">
    <tr>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">query_label</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">reference_label</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">distance</th>
        <th style="padding-left: 1em; padding-right: 1em; text-align: center">rank</th>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Victoria Beckham</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1.1102230246251565e-16</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">1</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">David Beckham</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.5481696102632145</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">2</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Stephen Dow Beckham</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.7849867068283364</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">3</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Mel B</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.8095855234085036</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">4</td>
    </tr>
    <tr>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">Caroline Rush</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">0.81982642291868</td>
        <td style="padding-left: 1em; padding-right: 1em; text-align: center; vertical-align: top">5</td>
    </tr>
</table>
[5 rows x 4 columns]<br/>
</div>




```python

```
