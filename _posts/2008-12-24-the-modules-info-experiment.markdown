---
layout: post
title: The module's info experiment
date: '2008-12-24'
tags:
- newkde
- newsuse
- software
- suse
- yast
---

This experiment is an idea from [Stano](http://en.opensuse.org/User:Visnov). When he saw us hacking and trying new approaches for the control center code base (in order to allow a different design) he sent me this ycp snippet.&nbsp;

```
import "Mode";
Mode::SetMode( "autoinst_config");

string m = (string) WFM::Args(0);
string p = (string) WFM::Args(1);

y2milestone( "Writing the result of '%1' to '%2'", m, p);
any a = WFM::call( m, ["Read"] );

any res = WFM::call( m, ["Summary"] );
y2milestone( "%1", res);

SCR::Write( .target.string, p, res);
```

}

This snippet would called with two parameters, A and B, would run a ycp client in [“auto” mode](http://forgeftp.novell.com/yast/doc/SL11.1/autoinstall/devel/ar01s03.html) and write a summary of the configuration in file B.

This would allow in the control panel to display “status” information about the modules before launching them.

To test the concept, I hacked a quick YModuleInfoProvider class that hooked into the Qt::Tooltip role of the YQModulesModel. I filled the implementation of this class with a hacky call to yast2, calling the ycp snippet and reading back the temporal file.

Ideally, the status should be shown in the icon itself or in a “Dolphin” like status bar. However, I just wanted to see it, so I put them in tooltips for now (Not that I want to keep them there).

And yes!, it works, you can see the status of some modules just h-overing them with your mouse:

![module status 1]({{ site.baseurl }}/assets/7-module-status-images/posts/1.png)

![module status 1]({{ site.baseurl }}/assets/images/posts/7-module-status-2.png)

![module status 1]({{ site.baseurl }}/assets/images/posts/7-module-status-3.png)

I would like to replace the implementation of YModuleInfoProvider with code using the YaST2 component API instead of an external call, just to see if it is cleaner, faster and whether we have more control. Stano helped me making this snippet work:

```
using namespace std;

int main()
{
    Y2Component *c = Y2ComponentBroker::createServer("wfm");

    if ( ! c )
    { cout << "error talking to wfm" << endl; return 1; }

    Y2Component *mode = Y2ComponentBroker::getNamespaceComponent("Mode");

    if ( ! mode )
    { cout << "error finding component" << endl; return 1; }

    Y2Namespace *ns = mode->import("Mode");

    if ( ! ns )
    { cout << "error importing Mode namespace" << endl; return 1; }

    Y2Function* fnc = ns->createFunctionCall("SetMode", Type::fromSignature("void(string)") );

    if ( ! fnc )
    { cout << "error calling SetMode" << endl; return 1; }

    fnc->appendParameter( YCPString( "autoinst_config" ) );
    fnc->finishParameters();
    fnc->evaluateCall();

    cout << "Done call!" << endl;
    fnc = ns->createFunctionCall("mode", Type::fromSignature("string(void)") );
    if ( ! fnc )
    { cout << "error calling mode" << endl; return 1; }

    YCPValue res = fnc->evaluateCall();
    cout << res->toString() << endl;

    return 0;
}
```

However, for this to work, it requires a small bug to be fixed in core. So I guess I will replace the implementations once this patch is in.

