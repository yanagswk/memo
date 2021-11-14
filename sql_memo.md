# todo
- alter table alter table テーブル名 add constraint 制約名（※削除する際に使用のadd constraintって？？

- 親テーブルを削除するときは、外部キーが設定されている、子テーブルを削除してから、削除する。  on delete restrictになっていると、外部キー制約で削除できなくする。

- truncateでもdeleteと同じように、レコードの削除ができるが、以下の特徴がある。
    - rollbackで戻せない (deleteはrollbackで戻せる。)
    - deleteより高速
    - where句は使用不可
    - auto_incrementは初期値となる (deleteはauto_incrementは初期化されない。)

- 選択されているデータベース確認コマンド  
  - select database();  

- テーブルのSQL確認  
  - show create table テーブル名;  
  - show create table テーブル名\G; (縦に表示)  

- テーブルの詳細なカラム情報  
```sql
show full columns from テーブル名
```

- カラムの順番を変えたい  
```sql
alter table テーブル名 
modify 移動したいカラム名 
text after 移動したいカラム名の上にくるカラム名;
```

- 外部キーのon deleteにcascadeを設定したら、主キーのレコードが削除されたら、外部キーのレコードも削除される。  


# テーブル結合  
(参考)[https://qiita.com/ngron/items/db4947fb0551f21321c0#left-join%E5%B7%A6%E5%A4%96%E9%83%A8%E7%B5%90%E5%90%88]  

内部結合・・・ 結合すべき行が見つからなかった場合にその行は消滅する。  
外部結合・・・ 結合すべき行が見つからなくても強制的に表示し、全ての値がNULLである行を生成して結合する  

## inner join  (内部結合)  
どちらかのテーブルに存在しない場合は結合結果から消滅される  
```sql
select articles.id, articles.title, articles.body, articles.user_id, users.name 
from articles
inner join users
on articles.user_id = users.id ;
```

## left join  (外部結合)
左の行は強制的に全て表示し、右テーブルには全ての値がNULLである行を生成して結合
```sql
select articles.id, articles.title, articles.body, articles.user_id, users.name 
from articles
left join users
on articles.user_id = users.id ;
```  

## 多対多の結合 (中間テーブル)  
tagsテーブル  
articlesテーブル  
article_tagテーブル (中間テーブル)
```sql
select tags.id,tags.name, articles.id, articles.title, articles.body 
from tags
left join article_tag
on tags.id = article_tag.tag_id 
left join articles
on article_tag.tag_id = tags.id ;
```
