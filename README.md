# 阿里云解析ddns更新shell脚本
修改自https://github.com/h46incon/AliDDNSBash<br/>
修改了系统变量运行环境，使原来适用openwrt的bash脚本适用于x86 ubuntu<br/>
ubuntu 16.04 测试通过<br/>
# 使用方法
# 安装依赖
首先需要一个shell

然后安装 bind-dig，curl，openssl-util。

# 修改脚本的setting代码段
其中DomainRecordId不清楚的话暂时不用修改，DNSServer修改为你在万网上使用的DNS服务器。
如:
    
    AccessKeyId="MyID"
    AccessKeySec="MySecret"
    DomainRecordId="00000"
    DomainRR="www"
    DomainName="example.com"
    DomainType="A"
    DNSServer="dns9.hichina.com"
    
如果不清楚DomainRecordId的话，修改main函数，在里面调用describe_record，如：

    main()
	    {
		    describe_record
		    #update_record
	    }

然后执行这个脚本。如果没问题的话，就能获取到域名的所有解析记录的列表了：

    {"PageNumber":1,"TotalCount":1,"PageSize":1,"RequestId":"0000","DomainRecords":{"Record":[{"RR":"www","Status":"ENABLE","Value":"8.8.8.8","RecordId":"21332133","Type":"A","DomainName":"example.com","Locked":false,"Line":"default","TTL":"600"},]}}HttpCode:200
上面的结果中，RecordId为21332133。得到结果后再修改DomainRecordId为正确的值。

# 修改main函数：
	    main()
	    {
		    #describe_record
		    update_record
	    }
执行脚本即可。脚本会在本机IP地址和当前域名解析设置不同的时候调用API更新设置。
