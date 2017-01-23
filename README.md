# minecraft-custom-items-code
java code using eclipse mars for creating minecraft custom items

- **For further instructions use:** http://minecraft.gamepedia.com/Mods/Creating_mods
- **Package and superclass names and creation sequence is the following:** ![79](https://cloud.githubusercontent.com/assets/22712175/22217483/e09d4fd0-e1ab-11e6-8862-1b6fb9ce10dd.png)

- **modid creation code:** Used for creating your mods name.To insert into game use /get modid:"name of your mod" through minecrafts's command line.

@Mod(modid = Reference.MOD_ID, name = Reference.NAME, version = Reference.VERSION, acceptedMinecraftVersions = Reference.ACCEPTED_VERSIONS)
public class tutorial {
	
	@Instance
	public static tutorial Instance; 
    
	@SidedProxy(clientSide = Reference.CLIENT_PROXY_CLASS, serverSide = Reference.SERVER_PROXY_CLASS)
	public static CommonProxy proxy;
	
	public static final CreativeTabs CREATIVE_TAB = new MyTab();
	
	@EventHandler
	public void preInit(FMLPreInitializationEvent event)
	{
		System.out.println("Pre Init");
		
		ModItems.init();
		ModItems.register();
		
		ModBlocks.init();
		ModBlocks.register();
    }

	@EventHandler
	public void init(FMLInitializationEvent event)
	{
		System.out.println("Init");
		proxy.init();
		
		ModCrafting.register();
    }
	@EventHandler
	public void postInit(FMLPostInitializationEvent event)
	{
		System.out.println("Post Init");
    }
	
}


- ** reference class code:** contains all your custom items and gives them a name to insert them into game.Use /get item"your items name".

public class Reference {

	public static final String MOD_ID = "dtm";
	public static final String NAME = "Dovahkiin's Mod";
	public static final String VERSION = "1.0";
	public static final String ACCEPTED_VERSIONS = "[1.9.4]";
	
	public static final String CLIENT_PROXY_CLASS = "Dovahkiin.tutorial.proxy.ClientProxy";
	public static final String SERVER_PROXY_CLASS = "Dovahkiin.tutorial.proxy.ServerProxy";
	
	public static enum TutorialItems {
		CHEESE("cheese","ItemCheese"),
		ANDURIL("anduril","ItemAnduril"),
		RUSK("rusk","ItemRusk"),
		CHEESE_AND_RUSK("cheese_and_rusk","ItemCheeseRusk");
		
		private String unlocalizedName;
		private String registryName;
		
		TutorialItems(String unlocalizedName, String registryName){
		    this.unlocalizedName = unlocalizedName;
			this.registryName = registryName;
		}
		
		public String getUnlocalizedName() {
			return unlocalizedName;
		}
		
		
		public String getRegistryName() {
			return registryName;
		}
		
	}
	
	public static enum TutorialBlocks {
		CHEESE("cheese", "BlockCheese"),
		JAR("jar", "BlockJar");
		
		private String unlocalizedName;
		private String registryName;
		
		TutorialBlocks(String unlocalizedName, String registryName){
		    this.unlocalizedName = unlocalizedName;
			this.registryName = registryName;
		}
		
		public String getUnlocalizedName() {
			return unlocalizedName;
		}
		
		
		public String getRegistryName() {
			return registryName;
		}
	}
}

- **block creation code:** 

public class ModBlocks {
	
	public static Block cheese;
	public static Block jar;

	public static void init() {
	      cheese = new BlockCheese();
	      jar = new BlockJar();
	}
	
	public static void register() {
		registerBlock(cheese);
		registerBlock(jar);
	}
	
	private static void registerBlock(Block block){
		GameRegistry.register(block);
		ItemBlock item = new ItemBlock(block);
		item.setRegistryName(block.getRegistryName());
		GameRegistry.register(item);
	}
	
	public static void registerRenders() {
		registerRender(cheese);
		registerRender(jar);
	}
	
	private static void registerRender(Block block) {
		Minecraft.getMinecraft().getRenderItem().getItemModelMesher().register(Item.getItemFromBlock(block), 0, new ModelResourceLocation(block.getRegistryName(), "inventory"));
	}
}

- **item registeration code:**

public class ModItems {
	
	public static Item cheese;
	public static Item anduril;
	public static Item rusk;
	public static Item cheese_and_rusk;
	
	public static void init() {
	      cheese = new ItemCheese();
	      anduril = new ItemAnduril();
	      rusk = new ItemRusk();
	      cheese_and_rusk = new ItemCheeseRusk();
	}
	
	public static void register() {
		GameRegistry.register(cheese);
		GameRegistry.register(anduril);
		GameRegistry.register(rusk);
		GameRegistry.register(cheese_and_rusk);
	}
	
	public static void registerRenders() {
		registerRender(cheese);
		registerRender(anduril);
		registerRender(rusk);
		registerRender(cheese_and_rusk);
	}
	
	private static void registerRender(Item item) {
		Minecraft.getMinecraft().getRenderItem().getItemModelMesher().register(item, 0, new ModelResourceLocation(item.getRegistryName(), "inventory"));
	}
	
}

- **recipy creation code:**

public class ModCrafting {
	
	public static void register(){
		GameRegistry.addShapedRecipe(new ItemStack(ModBlocks.cheese), "CCC", "CCC", "CCC", 'C', ModItems.cheese);	
		GameRegistry.addShapedRecipe(new ItemStack(ModBlocks.jar), " C ", "GGG", "CCC",'C', Blocks.COAL_BLOCK, 'G', new ItemStack(Blocks.STAINED_GLASS, 1, 0));
		GameRegistry.addShapedRecipe(new ItemStack(ModItems.cheese_and_rusk), "C", "R", 'C', ModItems.cheese, 'R', ModItems.rusk);
	}
}

- **custom tab code:**

public class MyTab extends CreativeTabs {
	
	public MyTab(){
		super("TabOfMine");
	}

	@Override
	public Item getTabIconItem() {
		return ModItems.cheese;
	}
}

- **adding proxy's:**

client:

public class ClientProxy implements CommonProxy {

	@Override
	public void init() {
		ModItems.registerRenders();
		ModBlocks.registerRenders();
	}
	
}

common:

public interface CommonProxy {
	
    public void init();
}

server:

public class ServerProxy implements CommonProxy {

	@Override
	public void init() {}
	
 }
 
 - **If there are any problems for the above contact me:dovahkiin.dragonborn3@hotmail.com**
