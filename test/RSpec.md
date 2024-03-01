# RSpec について

公式記事 : https://rspec.info/

<br>

## **【あるメソッドのスタブ化】**

とあるテストであるメソッドの返り値のデータ数が 100 件を超えた場合は、そのプロダクトでは上限エラーにする仕様があった場合に、テストコードを書く際に 100 件のテストデータを投入しても良いが、擬似的にデータ取得メソッドの返り値を 100 件に（スタブ化）する方法

`test_data = TestClass.get(test_ids, **fetch_filter_option)`

みたいなメソッドを擬似的に 100 件データを返すようにするには

```ruby
context '取得データが100件以上の場合' do
      before do
        dummy_intstance = spy('TestClass')
        dummy_data = Array.new(100, dummy_intstance)
        allow(AdpArticle).to receive(:get).and_return(dummy_data)
      end

      example 'httpステータス400が返される' do
        get_data
        expect(last_response.status).to eq 200
      end
    end
```
