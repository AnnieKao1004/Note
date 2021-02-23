# Override validate message
1.  Global
```javascript
const validateMessages = {
  required: "'${name}' is Required!",
  // ...
};

<ConfigProvider form={{ validateMessages }}>
  <Form />
</ConfigProvider>;
```
2.  Form.Item rule API  
`rules = Rule []`  
`type Rule = RuleConfig | (Form: FormInstance) => RuleConfig;`

```javascript
// Rule = RuleConfig

<Form.Item
  rules={[{
    required: true,
    message: 'This field is required'
  }]}  
>
  <Input />
</Form.Item>
```
```javascript
// Rule = (Form: FormInstance) => RuleConfig
// Custom validator, check confirm password value === password value

<Form.Item
  label="Confirm Password"
  rules={[({ getFieldValue }) => ({
    validator(_, value) {
      if (getFieldValue('password') === value) {
        return Promise.resolve();
      }
      return Promise.reject('Passwords do not match!');
    },
  })
 ]}  
>
  <Input />
</Form.Item>
```


