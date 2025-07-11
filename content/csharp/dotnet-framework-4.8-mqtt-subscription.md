---
title: ".NET Framework 4.8 MQTT Subscription and Handling"
---


# 1、安装 nuget 依赖

```
MQTTnet
```

# 2、测试代码

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Threading;
using MQTTnet;
using MQTTnet.Client;
using MQTTnet.Formatter;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

namespace WpfApp1
{
    class MQTT
    {
        // MQTT 配置
        private const string username = "username";
        private const string password = "password==";
        private const string topic = "testtopic/#";

        // MQTT 连接
        private IMqttClient mqttClient;

        public async void InitMQTT(Dispatcher dispatcher)
        {
            // 连接 MQTT
            var options = new MqttClientOptions
            {
                ClientId = Guid.NewGuid().ToString("D"),
                ProtocolVersion = MqttProtocolVersion.V500,
                ChannelOptions = new MqttClientTcpOptions
                {
                    Server = "mq.test.com",
                    Port = 1883
                    },
                Credentials = new MqttClientCredentials(username, Encoding.UTF8.GetBytes(password)),
                CleanSession = true,
                KeepAlivePeriod = TimeSpan.FromSeconds(60)
                };
            if (null != mqttClient)
            {
                await mqttClient.DisconnectAsync();
                mqttClient = null;
            }
            mqttClient = new MqttFactory().CreateMqttClient();

            // 处理 MQTT 消息
            mqttClient.ApplicationMessageReceivedAsync += e =>
            {
                // 序列化消息
                var str = Encoding.UTF8.GetString(e.ApplicationMessage.Payload);
                JObject jsonD = (JObject)JsonConvert.DeserializeObject(str);
                // 获取事件
                var eventCode = jsonD["event"];
                return Task.CompletedTask;
            };

            // 处理 MQTT 断开
            mqttClient.DisconnectedAsync += e =>
            {
                Debug.WriteLine($@"{DateTime.Now} Client is Connected:  IsSessionPresent:{e.ConnectResult}");

                // 启动 MQTT
                await mqttClient.ConnectAsync(options);

                // 监听话题
                await mqttClient.SubscribeAsync(topic);

                return Task.CompletedTask;
            };

            // 启动 MQTT
            await mqttClient.ConnectAsync(options);

            // 监听话题
            await mqttClient.SubscribeAsync(topic);
        }

    }
}
```
