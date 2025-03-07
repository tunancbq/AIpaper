# 复旦&中科院联袂打造，InternVideo2重塑视频理解新标杆，跨模态学习引领行业风向 

## 引言：视频理解的新篇章——InternVideo2的介绍

随着视频内容在日常生活中的普及，视频理解技术的重要性日益凸显。视频不仅包含丰富的视觉信息，还蕴含着动态变化和多模态元素，如音频和文本。这些特性使得视频成为一个复杂的数据类型，对其进行深入理解和分析是一项挑战。近年来，随着大型语言模型（LLM）和多模态大型语言模型（MLLM）的发展，视频理解领域迎来了新的发展机遇。这些模型通过学习世界模型，为视频嵌入提供了新的视角，从而推动了视频理解技术的进步。

在此背景下，我们介绍了一种新的视频基础模型（ViFM）——InternVideo2。InternVideo2采用了渐进式训练范式，统一了不同的自监督或弱监督学习框架，包括遮蔽视频标记重建、跨模态对比学习和下一个标记预测。这些训练阶段引导模型通过不同的前置任务捕获不同层次的结构和语义信息。在数据层面，我们优先考虑时空一致性，通过语义分割视频并生成视频-音频-语音字幕，改善了视频与文本之间的对齐。我们对InternVideo2的数据和模型规模进行了扩展。通过广泛的实验，我们验证了我们的设计，并展示了InternVideo2在超过60个视频和音频任务上取得了最先进的性能。值得注意的是，我们的模型在各种视频相关的字幕、对话和长视频理解基准测试中表现优异，凸显了其在推理和理解长时间上下文方面的能力。

**论文标题、机构、论文链接和项目地址**

- 论文标题：INTERNVIDEO2: SCALING VIDEO FOUNDATION MODELS FOR MULTIMODAL VIDEO UNDERSTANDING
- 机构：OpenGVLab, Shanghai AI Laboratory, Zhejiang University, The University of Hong Kong, Nanjing University, Fudan University, Shenzhen Institutes of Advanced Technology, Chinese Academy of Sciences
- 论文链接：[https://arxiv.org/pdf/2403.15377.pdf](https://arxiv.org/pdf/2403.15377.pdf)
- 项目地址：[https://github.com/OpenGVLab/InternVideo2](https://github.com/OpenGVLab/InternVideo2)

## InternVideo2模型架构：三阶段的渐进式学习方法

InternVideo2模型采用了一种渐进式学习方法，该方法包括三个阶段：遮蔽视频令牌重建、跨模态对比学习和下一个令牌的预测。这些阶段旨在提高模型的时空感知能力，将其与其他模态的语义对齐，并通过下一个令牌预测来增强其世界模型。

**1. 遮蔽视频令牌重建**

在遮蔽视频令牌重建的初始阶段，模型学习重建被遮蔽的视频令牌，使视频编码器能够发展基本的时空感知。为了估计被遮蔽的令牌，使用了不同训练的视觉编码器（InternViT和VideoMAE-g）作为代理。这一阶段的学习目标是通过重建剩余的令牌来形成，其中包括最小化相关未遮蔽令牌的均方误差（MSE）。

**2. 跨模态对比学习**

在多模态学习的下一阶段，架构扩展为包括音频和文本编码器。这不仅提高了视频和文本之间的对齐，还使InternVideo2能够处理视频-音频任务。通过结合这些额外的模态，模型对视频的理解得到了丰富，并与音频提供的语义对齐。

**3. 下一个令牌的预测**

在下一个令牌预测阶段，利用视频中心对话系统和相应的指令微调数据集来训练InternVideo2。这一迁移学习过程使模型能够从LLM和其他知识中受益。通过将InternVideo2连接到LLM，视频编码器通过下一个令牌预测训练进一步更新，增强了其生成上下文相关下一个令牌的能力。

## 数据处理的创新：时空一致性的重要性

在数据处理方面，InternVideo2强调了时空一致性的重要性。通过语义分割视频并生成视频-音频-语音字幕，改进了视频和文本之间的对齐。

**1. 视频剪辑的语义分割**

为了保持时空一致性，使用AutoShot模型代替传统的SceneDet滤镜来分割视频剪辑。AutoShot基于时间语义变化而不是像素差异来预测边界，从而生成语义完整的剪辑，避免混入不一致的上下文。

**2. 视频、音频和语音字幕的生成与融合**

在MVid数据集中，视频来自多个来源，包括YouTube和其他匿名来源，以提高数据集的多样性。对于视频数据集，首先保留超过2秒的剪辑。对于超过30秒的视频剪辑，如果剪辑中的片段来自同一镜头，则随机选择一个30秒的片段。此外，还自动为MVid的视觉、音频和语音生成字幕，然后使用LLM校正并融合它们，以便训练使用。

## 实验验证：跨越70个视频理解任务的表现

**1. 动作识别**

在动作识别方面，InternVideo2在多个数据集上进行了测试，包括Kinetics（K400、K600和K700）、Moments in Time V1（MiT）、Something-Something V2（SSv2）、UCF、HMDB、Charades、ActivityNet（ANet）和HACS。实验结果显示，InternVideo2在使用16帧的情况下，就能在这些数据集上取得新的最佳表现，超越了以往需要更高分辨率（例如224对比576）或模型集成的SOTA（State-of-the-Art）结果。例如，在MiT数据集上，InternVideo2-6B的表现超过了之前的SOTA，CoCa-g，达到了51.2%的准确率，比CoCa-g高出2.2%。在强调时间动态的Something-Something V2数据集上，InternVideo2-6B也以77.5%的准确率超越了MVD（77.3%）。此外，InternVideo2-6B在未裁剪视频分析上的表现也是顶尖的，例如在ActivityNet上达到了95.9%，在HACS上达到了97.0%。

**2. 视频-文本任务**

在视频-文本任务方面，InternVideo2在视频检索、视频字幕和多选视频问答（QA）等任务上进行了评估。在视频检索任务中，使用阶段2中的文本编码器，将视频表示与候选文本进行匹配。在多选视频问答任务中，使用阶段3中学习的VideoLLM进行测试。此外，InternVideo2还在音频任务上进行了测试，展示了其在音频和文本编码器上的优势。

**3. 视频中心对话**

在视频中心对话方面，InternVideo2在MVBench、VideoChatGPT-Eval和MoVQA等数据集上的表现突出，不仅在平均分数上超过了其他系统，而且在每个子任务上（详见补充材料）也表现出色，除了在VideoChatGPT-Eval上。这些结果表明，InternVideo2确实嵌入了部分世界模型的知识，至少与其他模型相比是这样。这也验证了学习可转移视频表示对当前视频相关的MLLM（多模态大型语言模型）的重要性。

## InternVideo2的优势：长视频理解与推理能力

InternVideo2在长视频理解和推理基准测试中表现出色，这突显了其在长时间上下文理解和推理能力方面的优势。在长视频或程序感知问答（QA）等复杂推理任务中，InternVideo2展现了其分析和推理一系列动作的能力。这些成果不仅证明了InternVideo2在视频感知、视频-语言对齐以及世界模型构建方面的卓越能力，还标志着其在多模态语言模型（MLLM）领域的各种基准测试中的顶级性能，有效地捕捉和理解视频内容。

## 模型的局限性与未来方向：固定输入分辨率和采样率的挑战

**1. 模型的局限性**

尽管InternVideo2在多模态视频理解任务中取得了显著的成绩，但它并没有引入新的训练方法或架构上的创新。相反，它利用现有的学习技术进行方案探索，同时专注于改进数据处理，以增强时空感知、语义对齐和基础知识嵌入。与先前的研究类似，InternVideo2仍然面临着固定输入分辨率、采样率和高度压缩的令牌的限制，这些限制了其表达丰富视频信息和捕捉细节的能力。

InternVideo2采用的渐进式学习方案在模型能力和训练计算之间取得了平衡。虽然同时学习三个优化目标在计算上是可行的，但当面临资源有限的情况时，可扩展性成为一个问题。

尽管InternVideo2在长视频理解和推理基准测试中表现出领先的性能，但它无法保证一个隐含的世界模型，以确保视觉推理的一致性。固定输入表示的内在约束，加上视觉推理任务的复杂性，呈现出在实现对视觉世界的全面和一致理解方面的挑战。

**2. 未来方向**

未来的研究方向可能包括开发新的模型架构和训练方法，以克服固定输入分辨率和采样率的限制。这可能涉及到探索更灵活的输入表示，以更好地捕捉视频内容的丰富性和细节。此外，研究人员可以探索如何有效地结合不同模态的信息，以进一步提高模型在多模态视频理解任务中的性能。

## 讨论与总结：InternVideo2在多模态视频理解中的潜力与影响

InternVideo2作为一种新型的视频基础模型，在多模态视频理解领域展现出了巨大的潜力。通过结合掩码视频令牌重建、视频-音频-文本对比学习以及下一个令牌预测，InternVideo2不仅在视频感知和视频-语言对齐方面表现出色，而且在模拟世界方面也有出色的表现。它在多模态语言模型（MLLM）领域的各种基准测试中的顶尖性能标志着其有效捕捉和理解视频内容的能力。这些经验性发现验证了InternVideo2作为未来探索视频理解的合格视频编码器的资格。

InternVideo2在视频相关对话和长视频理解方面的卓越性能，突显了其在各种世界模型研究和应用中的潜力。然而，我们也必须认识到，与其他基础模型一样，InternVideo2有可能嵌入其训练数据中存在的偏见，这些偏见可能由数据创建者的个人观点、偏好、价值观和视角以及所使用的训练语料库引起。这些偏见在AI模型中的存在可能会产生社会影响，并加剧现有的不平等或偏见。因此，在将InternVideo2部署到现实世界应用中时，必须仔细考虑潜在的影响，并采取积极措施来减轻偏见，确保公平性。

## 更广泛的影响：训练数据中的偏见问题及其社会影响

在构建和训练机器学习模型，尤其是视频理解模型如InternVideo2时，训练数据的选择和处理至关重要。这些数据不仅决定了模型的性能，还可能在模型中引入偏见，从而影响模型在现实世界中的应用和社会影响。

**1. 训练数据的多样性和代表性**

InternVideo2模型的训练数据包括来自不同来源的视频，这些视频覆盖了从第一人称到第三人称的不同视角，时长短长不一，涉及多样的角色和场景。例如，K-Mash数据集包含了来自著名动作识别数据集的视频，而K-Mash2M则进一步从YouTube中精选了视频以增加多样性。此外，MVid数据集结合了视频、音频、语音信息及其文本描述，这些丰富的多模态信息有助于模型更好地理解和处理视频内容。

然而，尽管这些数据集的多样性和代表性有所提高，但仍然存在潜在的偏见风险。例如，如果视频数据集中某一类别的视频过多或过少，模型可能会在识别该类别的视频时表现出偏差。此外，数据集中的文化背景也可能影响模型的学习，例如MVid数据集中包含了一小部分中国数据，这可能会导致模型对特定文化背景的视频有更好的理解能力。

**2. 偏见的来源和影响**

训练数据中的偏见可能来源于数据创作者的个人观点、偏好、价值观和视角，以及所使用的训练语料库。例如，视频数据的采集、剪辑和注释过程中的主观性可能会导致某些群体或行为被不公正地表示或忽略。此外，使用的语言模型（如LLMs）和神经教师（如InternViT [Chen et al., 2023a] 和VideoMAE [Wang et al., 2023a]）也可能将它们自身的偏见传递给视频理解模型。

这些偏见在AI模型中的存在可能会在社会上产生影响，加剧现有的不平等或偏见。例如，如果InternVideo2在处理与性别、种族或年龄相关的视频内容时表现出偏差，可能会在输出中体现出不公平或歧视性的结果，从而在社会中强化训练数据中存在的社会偏见或刻板印象。

**3. 应对偏见的措施**

为了减轻训练数据中的偏见并确保公平性，需要采取积极的措施。这可能包括使用更加多样化和平衡的数据集、对数据进行仔细的审查和预处理以消除偏见，以及开发和应用算法来识别和纠正模型中的偏见。此外，对模型的输出进行监控和评估，以确保其在现实世界应用中不会产生不公正或歧视性的影响，也是非常重要的。

总之，训练数据中的偏见问题不仅影响模型的性能，还可能对社会产生深远的影响。因此，在开发和部署视频理解模型时，确保训练数据的质量和公平性是至关重要的。 

