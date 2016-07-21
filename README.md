# DynHook
Example library for how to dynamically/statically hook/intercept unmanaged functions and APIs

Example code for interepting COM call (except taken from unit test):

		hr = LoadTypeLibEx(CComBSTR("UnitTests.tlb"), REGKIND_REGISTER, &spTypeLib);
		ISC::GetISC().AddTypeLibrary(spTypeLib);

		p = std::auto_ptr<FakeImpl>(new FakeImpl());
		hook.HookInterface(reinterpret_cast<IUnknown*>(p.get()), __uuidof(ITestInterface), &listener);

		p->MethodInt(123);

		Assert::AreEqual("ITestInterface->MethodInt(123)", listener.GetTrace());

Example code for intercepting normal __stdcall unmanaged code (again taken from unit test project):

			s_hookManager.InstallDynamicHook(
				TestDynamicHook::StringFunction, "StringFunction", &s_parameters, &s_listener);

			TestDynamicHook::IntFunction(100, 200);				
			
			Assert::AreEqual("IntFunction(100, 200)", s_listener.GetTrace());
