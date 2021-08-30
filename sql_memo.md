# todo
- alter table alter table テーブル名 add constraint 制約名（※削除する際に使用のadd constraintって？？

- on update、on deleteとは？ 外部キー制約？？

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
alter table テーブル名 modify 移動したいカラム名 text after 移動したいカラム名の上にくるカラム名;
```

- 外部キーのon deleteにcascadeを設定したら、主キーのレコードが削除されたら、外部キーのレコードも削除される。
