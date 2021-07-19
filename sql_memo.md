# todo
- alter table alter table テーブル名 add constraint 制約名（※削除する際に使用のadd constraintって？？

- on update、on deleteとは？ 外部キー制約？？

- 親テーブルを削除するときは、外部キーが設定されている、子テーブルを削除してから、削除する。  on delete restrictになっていると、外部キー制約で削除できなくする。

- truncateでもdeleteと同じように、レコードの削除ができるが、以下の特徴がある。
    - rollbackで戻せない (deleteはrollbackで戻せる。)
    - deleteより高速
    - where句は使用不可
    - auto_incrementは初期値となる (deleteはauto_incrementは初期化されない。)
