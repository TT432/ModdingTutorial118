# 从 Json 到 BakedModel
闲出屁的人才会搞这个

Json 模型加载的位置：ModelBakery#processLoading。

Forge 很贴心的给了个继承他的东西 ForgeModelBakery。

然后如下写法就可以工作了。
```java
@Mod.EventBusSubscriber(bus = Mod.EventBusSubscriber.Bus.MOD, value = Dist.CLIENT)
public class BakedModelHandler {
    private static final ResourceLocation TEST_MACHINE_NAME = prefix("block/test");
    public static BakedModel testMachine;

    @SubscribeEvent
    public static void registerModelUnBake(ModelRegistryEvent e) {
        ForgeModelBakery.addSpecialModel(TEST_MACHINE_NAME);
    }

    @SubscribeEvent
    public static void onModelBake(ModelBakeEvent evt) {
        testMachine = evt.getModelRegistry().get(TEST_MACHINE_NAME);
    }

    private static ResourceLocation prefix(String path) {
        return new ResourceLocation(YOUR_MOD_ID, path);
    }
}
```
此处的 TEST_MACHINE_NAME 对应的是 YOUR_MOD_ID/models/block/test.json。

渲染时：
```java
poseStack.translate(0, 0.5, 0);
poseStack.translate(0.5, 0.5, 0.5);
poseStack.mulPose(Vector3f.YP.rotationDegrees((float) time % 360));
poseStack.translate(-0.5, -0.5, -0.5);
poseStack.translate(0F, (float) Math.sin(time / 20.0) * 0.05F, 0F);
BakedModel cube = BakedModelHandler.testMachine;
Minecraft.getInstance().getBlockRenderer().getModelRenderer()
        .renderModel(poseStack.last(), buffer, blockEntity.getBlockState(),
                cube, 1, 1, 1, packedLight, packedOverlay);
poseStack.popPose();
```