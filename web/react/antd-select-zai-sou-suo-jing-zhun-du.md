在使用select进行搜索时，出现了很多不符合搜索条件的Option，代码如下：

```
                  <Select
                        placeholder="选择客户"
                        style={{ width: 120 }}
                        onSearch={this.fetchUser}
                        showSearch
                        notFoundContent={fetching ? <Spin size="small" /> : null}
                        filterOption={false}
                        onChange={(value) => {
                            this.handleChange("cid", value || "");
                        }}

                        value={newData.cname}
                    >
                        {(customerData || []).map((x, i) => {
                            return (
                                <Option value={x.id} key={i}>{x.customer}</Option>
                            )
                        })}
                    </Select>
```

尝试了许多方法，异步、动态修改Option数据、filterOption都不行，默认的数据还是去不掉，如下图所示：

![](/assets/import.png)

符合要求的在最下面显示，上面的我一直怀疑是select默认的数据，一直无法去掉，最终我改好了，只是很简单的去掉Option中的value，代码如下：

```
                   <Select
                        placeholder="选择客户"
                        style={{ width: 120 }}
                        onSearch={this.fetchUser}
                        showSearch
                        notFoundContent={fetching ? <Spin size="small" /> : null}
                        filterOption={false}
                        onChange={(value) => {
                            this.handleChange("cname", cname[value] || "");
                            this.handleChange("cid", value || "");
                        }}

                        value={newData.cname}
                    >
                        {(customerData || []).map((x, i) => {
                            return (
                                <Option key={x.id}>{x.customer}</Option>
                            )
                        })}
                    </Select>
```

一脸懵逼。。。。。。

