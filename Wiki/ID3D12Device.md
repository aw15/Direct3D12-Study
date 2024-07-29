# ID3D12Device
`ID3D12Device`는 Direct3D 12 API의 핵심 인터페이스 중 하나로, 그래픽 디바이스와 관련된 자원과 상태를 관리하고, 다양한 그래픽 작업을 수행할 수 있는 기능을 제공합니다. 이 인터페이스는 Direct3D 12 애플리케이션에서 GPU와 상호작용하기 위한 주요 수단으로 사용됩니다.

## 사용 방식
    hr = m_d3dDevice->CheckFeatureSupport(D3D12_FEATURE_FEATURE_LEVELS, &featLevels, sizeof(featLevels));
    if (SUCCEEDED(hr))
    {
        m_d3dFeatureLevel = featLevels.MaxSupportedFeatureLevel;
    }
    else
    {
        m_d3dFeatureLevel = m_d3dMinFeatureLevel;
    }
    
그래픽 하드웨어와 드라이버가 특정 기능을 지원하는지 확인하는 데 사용됩니다. 이 메서드는 하드웨어의 다양한 기능 수준을 동적으로 쿼리할 수 있게 해주므로, 애플리케이션이 특정 기능에 의존할 수 있는지 확인하거나 적절한 대체 방법을 사용할 수 있도록 도와줍니다.

---
	m_d3dDevice->CreateCommandQueue(&queueDesc, IID_PPV_ARGS(m_commandQueue.ReleaseAndGetAddressOf()));
	
	m_d3dDevice->CreateCommandAllocator(D3D12_COMMAND_LIST_TYPE_DIRECT, IID_PPV_ARGS(m_commandAllocators[n].ReleaseAndGetAddressOf()));
	
	m_d3dDevice->CreateCommandList(0, D3D12_COMMAND_LIST_TYPE_DIRECT, m_commandAllocators[0].Get(), nullptr, IID_PPV_ARGS(m_commandList.ReleaseAndGetAddressOf()));

**커맨드 큐 생성**: GPU에서 실행할 명령을 제출하기 위한 큐를 생성합니다. 커맨드 큐는 렌더링, 복사, 컴퓨팅 등의 작업을 수행하는 커맨드 리스트를 GPU로 보내는 역할을 합니다.

**커맨드 리스트 및 커맨드 얼로케이터 생성**: GPU 명령을 작성하는 데 사용되는 커맨드 리스트와, 이를 효율적으로 관리하는 커맨드 얼로케이터를 생성합니다.

---

	m_d3dDevice->CreateFence(m_fenceValues[m_backBufferIndex], D3D12_FENCE_FLAG_NONE, IID_PPV_ARGS(m_fence.ReleaseAndGetAddressOf()));

**펜스(fence) 및 이벤트**: CPU와 GPU 간의 동기화를 위해 펜스와 이벤트 객체를 생성하고 관리합니다. 이를 통해 GPU 작업의 완료를 확인하거나 CPU와의 동기화를 유지할 수 있습니다.

---

	m_d3dDevice->CreateDescriptorHeap(&rtvDescriptorHeapDesc, IID_PPV_ARGS(m_rtvDescriptorHeap.ReleaseAndGetAddressOf()));
	
	m_d3dDevice->CreateRenderTargetView(m_renderTargets[n].Get(), &rtvDesc, rtvDescriptor);
	
	m_d3dDevice->CreateCommittedResource(
            &depthHeapProperties,
            D3D12_HEAP_FLAG_NONE,
            &depthStencilDesc,
            D3D12_RESOURCE_STATE_DEPTH_WRITE,
            &depthOptimizedClearValue,
            IID_PPV_ARGS(m_depthStencil.ReleaseAndGetAddressOf())
            ))
	
 **버퍼 및 텍스처 생성**: `ID3D12Device`는 버퍼, 텍스처, 그리고 기타 리소스를 생성하는 데 사용됩니다. 이러한 리소스는 GPU에서 데이터를 저장하거나 렌더링하는 데 사용됩니다.
 **리소스 뷰 생성**: 셰이더 리소스 뷰(SRV), 언오더드 액세스 뷰(UAV), 렌더 타겟 뷰(RTV) 및 깊이 스텐실 뷰(DSV)와 같은 리소스 뷰를 생성하여 셰이더와의 인터페이스를 제공합니다.

---

    HRESULT hr = device->CreateGraphicsPipelineState(&psoDesc,IID_GRAPHICS_PPV_ARGS(pPipelineState));
        
	device->CreateComputePipelineState(&desc, IID_GRAPHICS_PPV_ARGS(pso.GetAddressOf()))

**그래픽 파이프라인 상태 객체**: 셰이더, 렌더 타겟 포맷, 블렌딩 상태, 래스터라이저 상태 등의 설정을 포함하는 그래픽 파이프라인 상태 객체(PSO)를 생성합니다. PSO는 GPU의 렌더링 설정을 정의하고, 이를 통해 최적의 성능을 제공합니다.

**컴퓨팅 파이프라인 상태 객체**: 컴퓨팅 작업을 위한 파이프라인 상태 객체를 생성합니다.