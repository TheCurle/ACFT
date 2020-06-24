# Events

There are many arcane secrets to the world of modding.
The Event Bus, and the Registry are the two that most adventurers fault at. 

As explained before, events are Forge's way of keeping everyone organised. They replace the ancient ways of every single mod replacing core game files, so that now Forge is the only thing modifying files. When the game reaches certain waypoints, Forge hooks in and sends out a signal to every mod listening, that now is the time to do what they wanted to do. There are events for all kinds of things. Here is a comprehensive list of events in 1.15.2, and what they are for.
```
====================================================

/**
 * Fired on {@link Dist#CLIENT} when {@link RecipeManager} has all of its recipes synced from the server to the client (just after a client has connected),
 */
public class RecipesUpdatedEvent extends Event
{
    
    private final RecipeManager mgr;


====================================================

/**
 * Fired when the ModelManager is notified of the resource manager reloading.
 * Called after model registry is setup, but before it's passed to BlockModelShapes.
 */
// TODO: try to merge with ICustomModelLoader
public class ModelBakeEvent extends Event
{
    private final ModelManager modelManager;
    private final Map<ResourceLocation, IBakedModel> modelRegistry;
    private final ModelLoader modelLoader;

====================================================

/**
 * Fired when the {@link net.minecraftforge.client.model.ModelLoader} is ready to receive registrations
 */
public class ModelRegistryEvent extends Event

====================================================

/**
 * {@link FurnaceFuelBurnTimeEvent} is fired when determining the fuel value for an ItemStack. <br>
 * <br>
 * To set the burn time of your own item, use {@link Item#getBurnTime(ItemStack)} instead.<br>
 * <br>
 * This event is fired from {@link ForgeEventFactory#getItemBurnTime(ItemStack)}.<br>
 * <br>
 * This event is {@link Cancelable} to prevent later handlers from changing the value.<br>
 * <br>
 * This event does not have a result. {@link HasResult}<br>
 * <br>
 * This event is fired on the {@link MinecraftForge#EVENT_BUS}.
 **/
@Cancelable
public class FurnaceFuelBurnTimeEvent extends Event
{
    @Nonnull
    private final ItemStack itemStack;
    private int burnTime;

====================================================


public class RenderWorldLastEvent extends net.minecraftforge.eventbus.api.Event
{
    private final WorldRenderer context;
    private final MatrixStack mat;
    private final float partialTicks;
    private final Matrix4f projectionMatrix;
    private final long finishTimeNano;


=====================================================
/**
 * VillagerTradesEvent is fired during the {@link FMLServerAboutToStartEvent}.  It is used to gather the trade lists for each profession.
 * It is fired on the {@link MinecraftForge#EVENT_BUS}.
 * It is fired once for each registered villager profession.
 * Villagers pick two trades from their trade map, based on their level.
 * Villager level is increased by successful trades.
 * The map is populated for levels 1-5 (inclusive), so Map#get will never return null for those keys.
 * Levels outside of this range do nothing, as specified by {@link VillagerData#func_221128_d(int)} which is called before attempting to level up.
 * To add trades to the merchant, simply add new trades to the list. {@link BasicTrade} provides a default implementation.
 */
public class VillagerTradesEvent extends Event
{

    protected Int2ObjectMap<List<ITrade>> 
    
=====================================================

/**
 * This event is fired on the {@link net.minecraftforge.common.MinecraftForge#EVENT_BUS}
 * whenever a hand is rendered in first person.
 * Canceling the event causes the hand to not render.
 */
@Cancelable
public class RenderHandEvent extends Event
{
    private final Hand hand;
    private final MatrixStack mat;
    private final IRenderTypeBuffer buffers;
    private final int light;
    private final float partialTicks;
    private final float interpolatedPitch;
    private final float swingProgress;
    private final float equipProgress;
    @Nonnull
    private final ItemStack stack;

    
=====================================================


    public static class ModConfigEvent extends Event {
        private final ModConfig config;

=====================================================

public abstract class RenderLivingEvent<T extends LivingEntity, M extends EntityModel<T>> extends Event
{

    private final LivingEntity entity;
    private final LivingRenderer<T, M> renderer;
    private final float partialRenderTick;
    private final MatrixStack matrixStack;
    private final IRenderTypeBuffer buffers;
    private final int light;

    .Pre
    .Post

=====================================================

/**
 * This event is called when an item is rendered in an item frame.
 *
 * You can set canceled to do no further vanilla processing.
 */
@Cancelable
public class RenderItemInFrameEvent extends net.minecraftforge.eventbus.api.Event
{
    private final ItemStack item;
    private final ItemFrameEntity entityItemFrame;
    private final ItemFrameRenderer renderer;
    private final MatrixStack matrix;
    private final IRenderTypeBuffer buffers;
    private final int light;

    
=====================================================

/**
 * Use these events to register block/item
 * color handlers at the appropriate time.
 */
public abstract class ColorHandlerEvent extends Event
{
    public static class Block extends ColorHandlerEvent
    {
        private final BlockColors blockColors;

        public Block(BlockColors blockColors)
        {
            this.blockColors = blockColors;
        }

        public BlockColors getBlockColors()
        {
            return blockColors;
        }
    }

    public static class Item extends ColorHandlerEvent
    {
        private final ItemColors itemColors;
        private final BlockColors blockColors;

        public Item(ItemColors itemColors, BlockColors blockColors)
        {
            this.itemColors = itemColors;
            this.blockColors = blockColors;
        }

        public ItemColors getItemColors()
        {
            return itemColors;
        }

        public BlockColors getBlockColors()
        {
            return blockColors;
        }
    }
}

=====================================================

/**
 * Called when a block's texture is going to be overlaid on the player's HUD. Cancel this event to prevent the overlay.
 */
@Cancelable
public class RenderBlockOverlayEvent extends Event
{

    public static enum OverlayType {
        FIRE, BLOCK, WATER
    }
    
    private final PlayerEntity player;
    private final MatrixStack mat;
    private final OverlayType overlayType;
    private final BlockState blockForOverlay;
    private final BlockPos blockPos;
    

=====================================================

public class BlockEvent extends Event
{
    private static final boolean DEBUG = Boolean.parseBoolean(System.getProperty("forge.debugBlockEvent", "false"));

    private final IWorld world;
    private final BlockPos pos;
    private final BlockState state;

===========================
/**
     * Fired when a block is about to drop it's harvested items. The {@link #drops} array can be amended, as can the {@link #dropChance}.
     * <strong>Note well:</strong> the {@link #harvester} player field is null in a variety of scenarios. Code expecting null.
     *
     * The {@link #dropChance} is used to determine which items in this array will actually drop, compared to a random number. If you wish, you
     * can pre-filter yourself, and set {@link #dropChance} to 1.0f to always drop the contents of the {@link #drops} array.
     *
     * {@link #isSilkTouching} is set if this is considered a silk touch harvesting operation, vs a normal harvesting operation. Act accordingly.
     *
     * @author cpw
     */
    public static class HarvestDropsEvent extends BlockEvent
    {
        private final int fortuneLevel;
        private final NonNullList<ItemStack> drops;
        private final boolean isSilkTouching;
        private float dropChance; // Change to e.g. 1.0f, if you manipulate the list and want to guarantee it always drops
        private final PlayerEntity harvester; // May be null for non-player harvesting such as explosions or machines

===========================
/**
     * Event that is fired when an Block is about to be broken by a player
     * Canceling this event will prevent the Block from being broken.
     */
    @Cancelable
    public static class BreakEvent extends BlockEvent
    {
        /** Reference to the Player who broke the block. If no player is available, use a EntityFakePlayer */
        private final PlayerEntity player;
        private int exp;

===========================
/**
     * Called when a block is placed.
     *
     * If a Block Place event is cancelled, the block will not be placed.
     */
    @Cancelable
    public static class EntityPlaceEvent extends BlockEvent
    {
        private final Entity entity;
        private final BlockSnapshot blockSnapshot;
        private final BlockState placedBlock;
        private final BlockState placedAgainst;

===========================
        /**
     * Fired when a single block placement triggers the
     * creation of multiple blocks(e.g. placing a bed block). The block returned
     * by {@link #state} and its related methods is the block where
     * the placed block would exist if the placement only affected a single
     * block.
     */
    @Cancelable
    public static class EntityMultiPlaceEvent extends EntityPlaceEvent
    {
        private final List<BlockSnapshot> blockSnapshots;

===========================
        /**
     * Fired when a physics update occurs on a block. This event acts as
     * a way for mods to detect physics updates, in the same way a BUD switch
     * does. This event is only called on the server.
     */
    @Cancelable
    public static class NeighborNotifyEvent extends BlockEvent
    {
        private final EnumSet<Direction> notifiedSides;
        private final boolean forceRedstoneUpdate;

===========================
/**
     * Fired to check whether a non-source block can turn into a source block.
     * A result of ALLOW causes a source block to be created even if the liquid
     * usually doesn't do that (like lava), and a result of DENY prevents creation
     * even if the liquid usually does do that (like water).
     */
    @HasResult
    public static class CreateFluidSourceEvent extends BlockEvent
    {

===========================

    /**
     * Fired when a liquid places a block. Use {@link #setNewState(IBlockState)} to change the result of
     * a cobblestone generator or add variants of obsidian. Alternatively, you  could execute
     * arbitrary code when lava sets blocks on fire, even preventing it.
     *
     * {@link #getState()} will return the block that was originally going to be placed.
     * {@link #getPos()} will return the position of the block to be changed.
     */
    @Cancelable
    public static class FluidPlaceBlockEvent extends BlockEvent
    {
        private final BlockPos liquidPos;
        private BlockState newState;
        private BlockState origState;

=========================== 
/**
     * Fired when a crop block grows.  See subevents.
     *
     */
    public static class CropGrowEvent extends BlockEvent
    {

==========
/**
         * Fired when any "growing age" blocks (for example cacti, chorus plants, or crops
         * in vanilla) attempt to advance to the next growth age state during a random tick.<br>
         * <br>
         * {@link Result#DEFAULT} will pass on to the vanilla growth mechanics.<br>
         * {@link Result#ALLOW} will force the plant to advance a growth stage.<br>
         * {@link Result#DENY} will prevent the plant from advancing a growth stage.<br>
         * <br>
         * This event is not {@link Cancelable}.<br>
         * <br>
         */
        @HasResult
        public static class Pre extends CropGrowEvent

=========
 /**
         * Fired when "growing age" blocks (for example cacti, chorus plants, or crops
         * in vanilla) have successfully grown. The block's original state is available,
         * in addition to its new state.<br>
         * <br>
         * This event is not {@link Cancelable}.<br>
         * <br>
         * This event does not have a result. {@link HasResult}<br>
         */
        public static class Post extends CropGrowEvent
        {
            private final BlockState originalState;

=========================== 
 /**
     * Fired when when farmland gets trampled
     * This event is {@link Cancelable}
     */
    @Cancelable
    public static class FarmlandTrampleEvent extends BlockEvent
    {

        private final Entity entity;
        private final float fallDistance;
===========================
/* Fired when an attempt is made to spawn a nether portal from
     * {@link net.minecraft.block.BlockPortal#trySpawnPortal(World, BlockPos)}.
     *
     * If cancelled, the portal will not be spawned.
     */
    @Cancelable
    public static class PortalSpawnEvent extends BlockEvent
    {
        private final NetherPortalBlock.Size size;


=====================================================
/**
 * Register all of your custom ModDimensons here, fired during server loading when
 * dimension data is read from the world file.
 *
 * Contains a list of missing entries. Registering an entry with the DimensionManger
 * will remove the matching entry from the missing list.
 */
public class RegisterDimensionsEvent extends Event
{
    private final Map<ResourceLocation, SavedEntry> missing;
    private final Set<ResourceLocation> keys;

=====================================================
/**
 * WorldEvent is fired when an event involving the world occurs.<br>
 * If a method utilizes this {@link Event} as its parameter, the method will
 * receive every child event of this class.<br>
 * <br>
 * {@link #world} contains the World this event is occurring in.<br>
 * <br>
 * All children of this event are fired on the {@link MinecraftForge#EVENT_BUS}.<br>
 **/
public class WorldEvent extends Event
{
    private final IWorld world;

=========================== 
/**
     * WorldEvent.Load is fired when Minecraft loads a world.<br>
     * This event is fired when a world is loaded in
     * {@link WorldClient#WorldClient(NetHandlerPlayClient, WorldSettings, int, EnumDifficulty, Profiler)},
     * {@link MinecraftServer#loadAllWorlds(String, String, long, WorldType, String)},
     * {@link IntegratedServer#loadAllWorlds(String, String, long, WorldType, String)}
     * {@link DimensionManager#initDimension(int)},
     * and {@link ForgeInternalHandler#onDimensionLoad(Load)}. <br>
     * <br>
     * This event is not {@link Cancelable}.<br>
     * <br>
     * This event does not have a result. {@link HasResult} <br>
     * <br>
     * This event is fired on the {@link MinecraftForge#EVENT_BUS}.<br>
     **/
    public static class Load extends WorldEvent
    {

=========================== 
/**
     * WorldEvent.Unload is fired when Minecraft unloads a world.<br>
     * This event is fired when a world is unloaded in
     * {@link Minecraft#loadWorld(WorldClient, String)},
     * {@link MinecraftServer#stopServer()},
     * {@link DimensionManager#unloadWorlds()},
     * {@link ForgeInternalHandler#onDimensionUnload(Unload)}. <br>
     * <br>
     * This event is not {@link Cancelable}.<br>
     * <br>
     * This event does not have a result. {@link HasResult} <br>
     * <br>
     * This event is fired on the {@link MinecraftForge#EVENT_BUS}.<br>
     **/
    public static class Unload extends WorldEvent

=========================== 
/**
     * WorldEvent.Save is fired when Minecraft saves a world.<br>
     * This event is fired when a world is saved in
     * {@link WorldServer#saveAllChunks(boolean, IProgressUpdate)},
     * {@link ForgeInternalHandler#onDimensionSave(Save)}. <br>
     * <br>
     * This event is not {@link Cancelable}.<br>
     * <br>
     * This event does not have a result. {@link HasResult} <br>
     * <br>
     * This event is fired on the {@link MinecraftForge#EVENT_BUS}.<br>
     **/
    public static class Save extends WorldEvent

=========================== 
/**
     * Called by WorldServer to gather a list of all possible entities that can spawn at the specified location.
     * If an entry is added to the list, it needs to be a globally unique instance.
     * The event is called in {@link WorldServer#getSpawnListEntryForTypeAt(EnumCreatureType, BlockPos)} as well as
     * {@link WorldServer#canCreatureTypeSpawnHere(EnumCreatureType, SpawnListEntry, BlockPos)}
     * where the latter checks for identity, meaning both events must add the same instance.
     * Canceling the event will result in a empty list, meaning no entity will be spawned.
     */
    @net.minecraftforge.eventbus.api.Cancelable
    public static class PotentialSpawns extends WorldEvent
    {
        private final EntityClassification type;
        private final BlockPos pos;
        private final List<SpawnListEntry> list;

=========================== 

    /**
     * Called by WorldServer when it attempts to create a spawnpoint for a dimension.
     * Canceling the event will prevent the vanilla code from running.
     */
    @net.minecraftforge.eventbus.api.Cancelable
    public static class CreateSpawnPosition extends WorldEvent
    {
        private final WorldSettings settings;
        
=====================================================
/**
 * WorldTypeEvent is fired when an event involving the world occurs.<br>
 * If a method utilizes this {@link Event} as its parameter, the method will
 * receive every child event of this class.<br>
 * <br>
 * {@link #worldType} contains the WorldType of the world this event is occurring in.<br>
 * <br>
 * All children of this event are fired on the {@link MinecraftForge#TERRAIN_GEN_BUS}.<br>
 **/
public class WorldTypeEvent extends Event
{
    private final WorldType worldType;

=========================== 
/**
     * BiomeSize is fired when vanilla Minecraft attempts to generate biomes.<br>
     * This event is fired during biome generation in
     * {@link GenLayer#initializeAllBiomeGenerators(long, WorldType, ChunkProviderSettings)}. <br>
     * <br>
     * {@link #originalSize} the original size of the Biome. <br>
     * {@link #newSize} the new size of the biome. Initially set to the {@link #originalSize}. <br>
     * If {@link #newSize} is set to a new value, that value will be used for the Biome size. <br>
     * <br>
     * This event is not {@link Cancelable}.<br>
     * <br>
     * This event does not have a result. {@link HasResult} <br>
     * <br>
     * This event is fired on the {@link MinecraftForge#TERRAIN_GEN_BUS}.<br>
     **/
    public static class BiomeSize extends WorldTypeEvent
    {
        private final int originalSize;
        private int newSize;

=====================================================
/**
 * BabyEntitySpawnEvent is fired just before a baby entity is about to be spawned. <br>
 * Parents will have disengaged their relationship. {@link @Cancelable} <br>
 * It is possible to change the child completely by using {@link #setChild(EntityAgeable)} <br>
 * This event is fired from {@link EntityAIMate#spawnBaby()} and {@link EntityAIVillagerMate#giveBirth()} <br>
 * <br>
 * {@link #parentA} contains the initiating parent entity.<br>
 * {@link #parentB} contains the secondary parent entity.<br>
 * {@link #causedByPlayer} contains the player responsible for the breading (if applicable).<br>
 * {@link #child} contains the child that will be spawned.<br>
 * <br>
 * This event is {@link Cancelable}.<br>
 * If this event is canceled, the child Entity is not added to the world, and the parents <br>
 * will no longer attempt to mate.
 * <br>
 * This event does not have a result. {@link HasResult}<br>
 * <br>
 * This event is fired on the {@link MinecraftForge#EVENT_BUS}.
 **/
@Cancelable
public class BabyEntitySpawnEvent extends net.minecraftforge.eventbus.api.Event
{
    private final MobEntity parentA;
    private final MobEntity parentB;
    private final PlayerEntity causedByPlayer;
    private AgeableEntity child;

=====================================================
/**
 * Parent type to all ModLifecycle events. This is based on Forge EventBus. They fire through the
 * ModContainer's eventbus instance.
 */
public class ModLifecycleEvent extends Event
{
    private final ModContainer container;
=====================================================
/**
 * Event fired when a LootTable json is loaded from json.
 * This event is fired whenever resources are loaded, or when the server starts.
 * This event will NOT be fired for LootTables loaded from the world folder, these are
 * considered configurations files and should not be modified by mods.
 *
 * Canceling the event will make it load a empty loot table.
 *
 */
@Cancelable
public class LootTableLoadEvent extends Event
{
    private final ResourceLocation name;
    private LootTable table;
    private LootTableManager lootTableManager;

=====================================================
/**
 * VillageSiegeEvent is fired just before a zombie siege finds a successful location in
 * {@link VillageSiege#trySetupSiege}, to give mods the chance to stop the siege.<br>
 * <br>
 * This event is {@link Cancelable}; canceling stops the siege.<br>
 * <br>
 * This event does not have a result. {@link HasResult}<br>
 * <br>
 * This event is fired on the {@link MinecraftForge#EVENT_BUS}.
 */
@Cancelable
public class VillageSiegeEvent extends Event
{
    private final VillageSiege siege;
    private final World world;
    private final PlayerEntity player;
    private final Vec3d attemptedSpawnPos;

=====================================================
/**
 * Fired on the client when {@link NetworkTagManager} has all of its tags synced from the server to the client (just after a client has connected).
 * Fired on the server when {@link NetworkTagManager} has read all tags from disk (during a data reload).
 * This event is fired on the {@link MinecraftForge#EVENT_BUS}
 * On the client, this event fires on the Client Thread.
 * On the server, this event may be fired on the Server Thread, or an async reloader thread.
 */
public class TagsUpdatedEvent extends Event
{
    
    private final NetworkTagManager manager;

=====================================================
public class PotionBrewEvent extends Event
{
    private NonNullList<ItemStack> stacks;


    /**
     * PotionBrewEvent.Pre is fired before vanilla brewing takes place.
     * All changes made to the event's array will be made to the TileEntity if the event is canceled.
     * <br>
     * The event is fired during the {@link BrewingStandTileEntity#brewPotions()} method invocation.<br>
     * <br>
     * {@link #stacks} contains the itemstack array from the TileEntityBrewer holding all items in Brewer.<br>
     * <br>
     * This event is {@link net.minecraftforge.eventbus.api.Cancelable}.<br>
     * If the event is not canceled, the vanilla brewing will take place instead of modded brewing.
     * <br>
     * This event does not have a result. {@link HasResult}<br>
     * <br>
     * This event is fired on the {@link MinecraftForge#EVENT_BUS}.<br>
     * <br>
     * If this event is canceled, and items have been modified, PotionBrewEvent.Post will automatically be fired.
     **/
    @net.minecraftforge.eventbus.api.Cancelable
    public static class Pre extends PotionBrewEvent 
    
    
       /**
     * PotionBrewEvent.Post is fired when a potion is brewed in the brewing stand.
     * <br>
     * The event is fired during the {@link BrewingStandTileEntity#brewPotions()} method invocation.<br>
     * <br>
     * {@link #stacks} contains the itemstack array from the TileEntityBrewer holding all items in Brewer.<br>
     * <br>
     * This event is not {@link net.minecraftforge.eventbus.api.Cancelable}.<br>
     * <br>
     * This event does not have a result. {@link HasResult}<br>
     * <br>
     * This event is fired on the {@link MinecraftForge#EVENT_BUS}.<br>
     **/
    public static class Post extends PotionBrewEvent

=====================================================

@net.minecraftforge.eventbus.api.Cancelable
public class ClientChatReceivedEvent extends net.minecraftforge.eventbus.api.Event
{
    private ITextComponent message;
    private final ChatType type;

=====================================================
public class InputEvent extends Event
{

    /**
     * A cancellable mouse event fired before key bindings are updated
     */
    @Cancelable
    public static class RawMouseEvent extends InputEvent
    {
        private final int button;
        private final int action;
        private final int mods;

/**
     * This event fires when a mouse input is detected.
     */
    public static class MouseInputEvent extends InputEvent
    {
        private final int button;
        private final int action;
        private final int mods;

        /**
     * This event fires when the mouse scroll wheel is used outside of a gui.
     */
    @Cancelable
    public static class MouseScrollEvent extends InputEvent
    {
        private final double scrollDelta;
        private final double mouseX;
        private final double mouseY;
        private final boolean leftDown;
        private final boolean middleDown;
        private final boolean rightDown;

        /**
     * This event fires when a keyboard input is detected.
     */
    public static class KeyInputEvent extends InputEvent
    {
        private final int key;
        private final int scanCode;
        private final int action;
        private final int modifiers;

        /**
     * This event fires when one of the keybindings that by default involves clicking the mouse buttons
     * is triggered.
     *
     * These key bindings are use item, pick block and attack keybindings. (right, middle and left mouse click)
     * In the case that these key bindings are re-bound to a keyboard key the event will still be fired as normal.
     */
    @Cancelable
    public static class ClickInputEvent extends InputEvent
    {
        private final int button;
        private final KeyBinding keyBinding;
        private final Hand hand;
        private boolean handSwing = true;

=====================================================
public class NetworkEvent extends Event
{
    private final PacketBuffer payload;
    private final Supplier<Context> source;
    private final int loginIndex;
    
===========================
     public static class ServerCustomPayloadEvent extends NetworkEvent
    {
=========================== 
public static class ClientCustomPayloadEvent extends NetworkEvent
    {
=========================== 
 public static class ServerCustomPayloadLoginEvent extends ServerCustomPayloadEvent {
   
=========================== 
public static class ClientCustomPayloadLoginEvent extends ClientCustomPayloadEvent {
     
=========================== 
public static class GatherLoginPayloadsEvent extends Event {
        private final List<NetworkRegistry.LoginPayload> collected;
        private final boolean isLocal;

=========================== 
public static class LoginPayloadEvent extends NetworkEvent {
    
=========================== 
/**
     * Fired when the channel registration (see minecraft custom channel documentation) changes. Note the payload
     * is not exposed. This fires to the resource location that owns the channel, when it's registration changes state.
     *
     * It seems plausible that this will fire multiple times for the same state, depending on what the server is doing.
     * It just directly dispatches upon receipt.
     */
    public static class ChannelRegistrationChangeEvent extends NetworkEvent {
        private final RegistrationChangeType changeType;

=====================================================
/**
 * DifficultyChangeEvent is fired when difficulty is changing. <br>
 * <br>
 * This event is fired via the {@link ForgeHooks#onDifficultyChange(EnumDifficulty, EnumDifficulty)}.<br>
 * <br>
 * This event is not {@link net.minecraftforge.eventbus.api.Cancelable}.<br>
 * <br>
 * This event does not have a result. {@link HasResult}<br>
 * <br>
 * This event is fired on the {@link MinecraftForge#EVENT_BUS}.
 **/
public class DifficultyChangeEvent extends net.minecraftforge.eventbus.api.Event
{
    private final Difficulty difficulty;
    private final Difficulty oldDifficulty;
=====================================================
/**
 * An event called whenever the selection highlight around blocks is about to be rendered.
 * Canceling this event stops the selection highlight from being rendered.
 */
@Cancelable
public class DrawHighlightEvent extends Event
{
    private final WorldRenderer context;
    private final ActiveRenderInfo info;
    private final RayTraceResult target;
    private final float partialTicks;
    private final MatrixStack matrix;
    private final IRenderTypeBuffer buffers;

=========================== 
/**
     * A variant of the DrawBlockHighlightEvent only called when a block is highlighted.
     */
    @Cancelable
    public static class HighlightBlock extends DrawHighlightEvent
    {
=========================== 
/**
     * A variant of the DrawBlockHighlightEvent only called when an entity is highlighted.
     * Canceling this event has no effect.
     */
    @Cancelable
    public static class HighlightEntity extends DrawHighlightEvent
    {
=====================================================
/**
 * This event is fired before and after a screenshot is taken
 * This event is fired on the {@link net.minecraftforge.common.MinecraftForge#EVENT_BUS}
 * This event is {@link Cancelable}
 *
 * {@link #screenshotFile} contains the file the screenshot will be/was saved to
 * {@link #image} contains the {@link NativeImage} that will be saved
 * {@link #resultMessage} contains the {@link ITextComponent} to be returned. If {@code null}, the default vanilla message will be used instead
 */
@Cancelable
public class ScreenshotEvent extends Event
{

    public static final ITextComponent DEFAULT_CANCEL_REASON = new StringTextComponent("Screenshot canceled");

    private NativeImage image;
    private File screenshotFile;

    private ITextComponent resultMessage = null;

=====================================================
/**
 * Fired when the enchantment level is set for each of the three potential enchantments in the enchanting table.
 * The {@link #level} is set to the vanilla value and can be modified by this event handler.
 *
 * The {@link #enchantRow} is used to determine which enchantment level is being set, 1, 2, or 3. The {@link #power} is a number
 * from 0-15 and indicates how many bookshelves surround the enchanting table. The {@link #itemStack} representing the item being
 * enchanted is also available.
 */
public class EnchantmentLevelSetEvent extends net.minecraftforge.eventbus.api.Event
{
    private final World world;
    private final BlockPos pos;
    private final int enchantRow;
    private final int power;
    @Nonnull
    private final ItemStack itemStack;
    private final int originalLevel;
    private int level;

=====================================================
/**
 * A set of events which are fired at various points during tooltip rendering.
 * <p>
 * Can be used to change the rendering parameters, draw something extra, etc.
 * <p>
 * Do not use this event directly, use one of the subclasses:
 * <ul>
 * <li>{@link RenderTooltipEvent.Pre}</li>
 * <li>{@link RenderTooltipEvent.PostBackground}</li>
 * <li>{@link RenderTooltipEvent.PostText}</li>
 * </ul>
 */
public abstract class RenderTooltipEvent extends net.minecraftforge.eventbus.api.Event
{
    @Nonnull
    protected final ItemStack stack;
    protected final List<String> lines;
    protected int x;
    protected int y;
    protected FontRenderer fr;


=========================== 
    /**
     * This event is fired before any tooltip calculations are done. It provides setters for all aspects of the tooltip, so the final render can be modified.
     * <p>
     * This event is {@link Cancelable}.
     */
    @net.minecraftforge.eventbus.api.Cancelable
    public static class Pre extends RenderTooltipEvent
    {
        private int screenWidth;
        private int screenHeight;
        private int maxWidth;


=========================== 
        /**
     * Events inheriting from this class are fired at different stages during the tooltip rendering.
     * <p>
     * Do not use this event directly, use one of its subclasses:
     * <ul>
     * <li>{@link RenderTooltipEvent.PostBackground}</li>
     * <li>{@link RenderTooltipEvent.PostText}</li>
     * </ul>
     */
    protected static abstract class Post extends RenderTooltipEvent
    {
        private final int width;
        private final int height;


=========================== 
        /**
     * This event is fired directly after the tooltip background is drawn, but before any text is drawn.
     */
    public static class PostBackground extends Post 
    {

=========================== 
        /**
     * This event is fired directly after the tooltip text is drawn, but before the GL state is reset.
     */
    public static class PostText extends Post
    {

=========================== 

         /**
     * This event is fired when the colours for the tooltip background are determined. 
     */
    public static class Color extends RenderTooltipEvent
    {
=====================================================
/**
 * Event classes for GuiScreen events.
 *
 * @author bspkrs
 */
@OnlyIn(Dist.CLIENT)
public class GuiScreenEvent extends Event
{
    private final Screen gui;

===========================
  public static class InitGuiEvent extends GuiScreenEvent
    {
        private Consumer<Widget> add;
        private Consumer<Widget> remove;

        private List<Widget> list;

=============
/**
         * This event fires just after initializing {@link GuiScreen#mc}, {@link GuiScreen#fontRenderer},
         * {@link GuiScreen#width}, and {@link GuiScreen#height}.<br/><br/>
         *
         * If canceled the following lines are skipped in {@link GuiScreen#setWorldAndResolution(Minecraft, int, int)}:<br/>
         * {@code this.buttonList.clear();}<br/>
         * {@code this.children.clear();}<br/>
         * {@code this.initGui();}<br/>
         */
        @Cancelable
        public static class Pre extends InitGuiEvent
        {

===========
/**
         * This event fires right after {@link GuiScreen#initGui()}.
         * This is a good place to alter a GuiScreen's component layout if desired.
         */
        public static class Post extends InitGuiEvent
        {

===========================
  public static class DrawScreenEvent extends GuiScreenEvent
    {
        private final int mouseX;
        private final int mouseY;
        private final float renderPartialTicks;

===========
/**
         * This event fires just before {@link GuiScreen#render(int, int, float)} is called.
         * Cancel this event to skip {@link GuiScreen#render(int, int, float)}.
         */
        @Cancelable
        public static class Pre extends DrawScreenEvent
        {

===========
/**
         * This event fires just after {@link GuiScreen#render(int, int, float)} is called.
         */
        public static class Post extends DrawScreenEvent
        {
=========================== 
/**
     * This event fires at the end of {@link GuiScreen#drawBackground(int)} and before the rest of the Gui draws.
     * This allows drawing next to Guis, above the background but below any tooltips.
     */
    public static class BackgroundDrawnEvent extends GuiScreenEvent
    {
=========================== 
/**
     * This event fires in {@link InventoryEffectRenderer#updateActivePotionEffects()}
     * when potion effects are active and the gui wants to move over.
     * Cancel this event to prevent the Gui from being moved.
     */
    @Cancelable
    public static class PotionShiftEvent extends GuiScreenEvent
    {
        
=========================== 
@Deprecated // Remove in 1.16
    public static class ActionPerformedEvent extends GuiScreenEvent
    {
        private Button button;
        private List<Button> buttonList;
        
===========
/**
         * This event fires once it has been determined that a GuiButton object has been clicked.
         * Cancel this event to bypass {@link GuiScreen#actionPerformed(GuiButton)}.
         * Replace button with a different button from buttonList to have that button's action executed.
         */
        @Cancelable
        public static class Pre extends ActionPerformedEvent
        {
===========
 /**
         * This event fires after {@link GuiScreen#actionPerformed(GuiButton)} provided that the active
         * screen has not been changed as a result of {@link GuiScreen#actionPerformed(GuiButton)}.
         */
        public static class Post extends ActionPerformedEvent
        {

=========================== 
 public static abstract class MouseInputEvent extends GuiScreenEvent
    {
        private final double mouseX;
        private final double mouseY;

=========================== 
 public static abstract class MouseClickedEvent extends MouseInputEvent
    {
        private final int button;

===========
/**
         * This event fires when a mouse click is detected for a GuiScreen, before it is handled.
         * Cancel this event to bypass {@link IGuiEventListener#mouseClicked(double, double, int)}.
         */
        @Cancelable
        public static class Pre extends MouseClickedEvent
        {
===========

        /**
         * This event fires after {@link IGuiEventListener#mouseClicked(double, double, int)} if the click was not already handled.
         * Cancel this event when you successfully use the mouse click, to prevent other handlers from using the same input.
         */
        @Cancelable
        public static class Post extends MouseClickedEvent
        {
=========================== 
public static abstract class MouseReleasedEvent extends MouseInputEvent
    {
        private final int button;

===========
       /**
         * This event fires when a mouse release is detected for a GuiScreen, before it is handled.
         * Cancel this event to bypass {@link IGuiEventListener#mouseReleased(double, double, int)}.
         */
        @Cancelable
        public static class Pre extends MouseReleasedEvent
        {
            public Pre(Screen gui, double mouseX, double mouseY, int button)
            {
                super(gui, mouseX, mouseY, button);
            }
        }
===========
/**
         * This event fires after {@link IGuiEventListener#mouseReleased(double, double, int)} if the release was not already handled.
         * Cancel this event when you successfully use the mouse release, to prevent other handlers from using the same input.
         */
        @Cancelable
        public static class Post extends MouseReleasedEvent
        {

=========================== 
public static abstract class MouseDragEvent extends MouseInputEvent
    {
        private final int mouseButton;
        private final double dragX;
        private final double dragY;
===========
/**
         * This event fires when a mouse drag is detected for a GuiScreen, before it is handled.
         * Cancel this event to bypass {@link IGuiEventListener#mouseDragged(double, double, int, double, double)}.
         */
        @Cancelable
        public static class Pre extends MouseDragEvent
        {
===========
 /**
         * This event fires after {@link IGuiEventListener#mouseDragged(double, double, int, double, double)} if the drag was not already handled.
         * Cancel this event when you successfully use the mouse drag, to prevent other handlers from using the same input.
         */
        @Cancelable
        public static class Post extends MouseDragEvent
        {
=========================== 
 public static abstract class MouseScrollEvent extends MouseInputEvent
    {
        private final double scrollDelta;

===========

        /**
         * This event fires when a mouse scroll is detected for a GuiScreen, before it is handled.
         * Cancel this event to bypass {@link IGuiEventListener#mouseScrolled(double)}.
         */
        @Cancelable
        public static class Pre extends MouseScrollEvent
        {
===========
 /**
         * This event fires after {@link IGuiEventListener#mouseScrolled(double)} if the scroll was not already handled.
         * Cancel this event when you successfully use the mouse scroll, to prevent other handlers from using the same input.
         */
        @Cancelable
        public static class Post extends MouseScrollEvent
        {
=========================== 
public static abstract class KeyboardKeyEvent extends GuiScreenEvent
    {
        private final int keyCode;
        private final int scanCode;
        private final int modifiers;

=========================== 
 public static abstract class KeyboardKeyPressedEvent extends KeyboardKeyEvent
    {
===========
 /**
         * This event fires when keyboard input is detected for a GuiScreen, before it is handled.
         * Cancel this event to bypass {@link IGuiEventListener#keyPressed(int, int, int)}.
         */
        @Cancelable
        public static class Pre extends KeyboardKeyPressedEvent
        {
===========
/**
         * This event fires after {@link IGuiEventListener#keyPressed(int, int, int)} if the key was not already handled.
         * Cancel this event when you successfully use the keyboard input to prevent other handlers from using the same input.
         */
        @Cancelable
        public static class Post extends KeyboardKeyPressedEvent
        {
=========================== 
 public static abstract class KeyboardKeyReleasedEvent extends KeyboardKeyEvent
    {
===========
/**
         * This event fires when keyboard input is detected for a GuiScreen, before it is handled.
         * Cancel this event to bypass {@link IGuiEventListener#keyReleased(int, int, int)}.
         */
        @Cancelable
        public static class Pre extends KeyboardKeyReleasedEvent
        {
===========
/**
         * This event fires after {@link IGuiEventListener#keyReleased(int, int, int)} if the key was not already handled.
         * Cancel this event when you successfully use the keyboard input to prevent other handlers from using the same input.
         */
        @Cancelable
        public static class Post extends KeyboardKeyReleasedEvent
        {
=========================== 
public static class KeyboardCharTypedEvent extends GuiScreenEvent
    {
        private final char codePoint;
        private final int modifiers;

===========
/**
         * This event fires when keyboard character input is detected for a GuiScreen, before it is handled.
         * Cancel this event to bypass {@link IGuiEventListener#charTyped(char, int)}.
         */
        @Cancelable
        public static class Pre extends KeyboardCharTypedEvent
        {
            public Pre(Screen gui, char codePoint, int modifiers)
            {
                super(gui, codePoint, modifiers);
            }
        }
===========
/**
         * This event fires after {@link IGuiEventListener#charTyped(char, int)} if the character was not already handled.
         * Cancel this event when you successfully use the keyboard input to prevent other handlers from using the same input.
         */
        @Cancelable
        public static class Post extends KeyboardCharTypedEvent
        
=====================================================
@Cancelable
public class RenderGameOverlayEvent extends Event
{
    private final float partialTicks;
    private final MainWindow window;
    private final ElementType type;
    
    public static class BossInfo extends Pre
    {
        private final ClientBossInfo bossInfo;
        private final int x;
        private final int y;
        private int increment;
    public static class Text extends Pre
    {
        private final ArrayList<String> left;
        private final ArrayList<String> right;

    public static class Chat extends Pre
    {
        private int posX;
        private int posY;
===========================
 public static class Pre extends RenderGameOverlayEvent
    {
=========================== 
    public static class Post extends RenderGameOverlayEvent
    {
=====================================================
/**
 * ClientChatEvent is fired whenever the client is about to send a chat message or command to the server. <br>
 * This event is fired via {@link ForgeEventFactory#onClientSendMessage(String)},
 * which is executed by {@link GuiScreen#sendChatMessage(String, boolean)}<br>
 * <br>
 * {@link #message} contains the message that will be sent to the server. This can be changed by mods.<br>
 * {@link #originalMessage} contains the original message that was going to be sent to the server. This cannot be changed by mods.<br>
 * <br>
 * This event is {@link Cancelable}. <br>
 * If this event is canceled, the chat message or command is never sent to the server.<br>
 * <br>
 * This event does not have a result. {@link HasResult}<br>
 * <br>
 * This event is fired on the {@link MinecraftForge#EVENT_BUS}.
 **/
@Cancelable
public class ClientChatEvent extends Event
{
    private String message;
    private final String originalMessage;
=====================================================
/**
 * Author: MachineMuse (Claire Semple)
 * Created: 6:07 PM, 9/5/13
 */
public class FOVUpdateEvent extends Event
{
    private final PlayerEntity entity;
    private final float fov;
    private float newfov;

=====================================================
/**
 * CommandEvent is fired after a command is parsed, but before it is executed.
 * This event is fired during the invocation of {@link Commands#handleCommand(CommandSource, String)}. <br>
 * <br>
 * {@link #parse} contains the instance of {@link ParseResults} for the parsed command.<br>
 * {@link #exception} begins null, but can be populated with an exception to be thrown within the command.<br>
 * <br>
 * This event is {@link Cancelable}. <br>
 * If the event is canceled, the execution of the command does not occur.<br>
 * <br>
 * This event does not have a result. {@link HasResult}<br>
 * <br>
 * This event is fired on the {@link MinecraftForge#EVENT_BUS}.<br>
 **/
@Cancelable
public class CommandEvent extends Event
{
    private ParseResults<CommandSource> parse;
    private Throwable exception;
=====================================================
public class TextureStitchEvent extends Event
{
    private final AtlasTexture map;

    public TextureStitchEvent(AtlasTexture map)
    {
        this.map = map;
    }

    public AtlasTexture getMap()
    {
        return map;
    }

===========================
/**
     * Fired when the TextureMap is told to refresh it's stitched texture.
     * Called before the {@link net.minecraft.client.renderer.texture.TextureAtlasSprite} are loaded.
     */
    public static class Pre extends TextureStitchEvent
    {
        private final Set<ResourceLocation> sprites;
===========================
 /**
     * This event is fired once the texture map has loaded all textures and
     * stitched them together. All Icons should have there locations defined
     * by the time this is fired.
     */
    public static class Post extends TextureStitchEvent
    {

=====================================================
/**
 * Implements {@link IGenericEvent} to provide filterable events based on generic type data.
 *
 * Subclasses should extend this if they wish to expose a secondary type based filter (the generic type).
 *
 * @param <T> The type to filter this generic event for
 */
public class GenericEvent<T> extends Event implements IGenericEvent<T>
{
    private Class<T> type;

=====================================================
/** ExplosionEvent triggers when an explosion happens in the world.<br>
 * <br>
 * ExplosionEvent.Start is fired before the explosion actually occurs.<br>
 * ExplosionEvent.Detonate is fired once the explosion has a list of affected blocks and entities.<br>
 * <br>
 * ExplosionEvent.Start is {@link Cancelable}.<br>
 * ExplosionEvent.Detonate can modify the affected blocks and entities.<br>
 * Children do not use {@link HasResult}.<br>
 * Children of this event are fired on the {@link MinecraftForge#EVENT_BUS}.<br>
 */
public class ExplosionEvent extends Event
{
    private final World world;
    private final Explosion explosion;
===========================
/** ExplosionEvent.Start is fired before the explosion actually occurs.  Canceling this event will stop the explosion.<br>
     * <br>
     * This event is {@link net.minecraftforge.eventbus.api.Cancelable}.<br>
     * This event does not use {@link HasResult}.<br>
     * This event is fired on the {@link MinecraftForge#EVENT_BUS}.<br>
     */
    @Cancelable
    public static class Start extends ExplosionEvent
    {
===========================
/** ExplosionEvent.Detonate is fired once the explosion has a list of affected blocks and entities.  These lists can be modified to change the outcome.<br>
     * <br>
     * This event is not {@link Cancelable}.<br>
     * This event does not use {@link HasResult}.<br>
     * This event is fired on the {@link MinecraftForge#EVENT_BUS}.<br>
     */
    public static class Detonate extends ExplosionEvent
    {
        private final List<Entity> entityList;

=====================================================
public class SoundEvent extends net.minecraftforge.eventbus.api.Event
{
    private final SoundEngine manager;
===========================
public static class SoundSourceEvent extends SoundEvent
    {
        private final ISound sound;
        private final SoundSource source;
        private final String name;

=====================================================
/**
 * 
 * AnvilUpdateEvent is fired when a player places items in both the left and right slots of a anvil.
 * If the event is canceled, vanilla behavior will not run, and the output will be set to null.
 * If the event is not canceled, but the output is not null, it will set the output and not run vanilla behavior.
 * if the output is null, and the event is not canceled, vanilla behavior will execute.
 */
@Cancelable
public class AnvilUpdateEvent extends Event
{
    @Nonnull
    private final ItemStack left;  // The left side of the input
    @Nonnull
    private final ItemStack right; // The right side of the input
    private final String name;     // The name to set the item, if the user specified one.
    @Nonnull
    private ItemStack output;      // Set this to set the output stack
    private int cost;              // The base cost, set this to change it if output != null
    private int materialCost; // The number of items from the right slot to be consumed during the repair. Leave as 0 to consume the entire stack.

=====================================================
public class TickEvent extends Event
{
    public enum Type {
        WORLD, PLAYER, CLIENT, SERVER, RENDER;
    }

    public enum Phase {
        START, END;
    }
    public final Type type;
    public final LogicalSide side;
    public final Phase phase;
    
===========================
 public static class ServerTickEvent extends TickEvent {
    
===========================
public static class ClientTickEvent extends TickEvent {
    
===========================
public static class WorldTickEvent extends TickEvent {
        public final World world;
    
===========================
public static class PlayerTickEvent extends TickEvent {
        public final PlayerEntity player;

===========================
 public static class RenderTickEvent extends TickEvent {
        public final float renderTickTime;

=====================================================
/**
 * WandererTradesEvent is fired during the {@link FMLServerAboutToStartEvent}.  It is used to gather the trade lists for the wandering merchant.
 * It is fired on the {@link MinecraftForge#EVENT_BUS}.
 * The wandering merchant picks a few trades from {@link TradeType#GENERIC} and a single trade from {@link TradeType#RARE}.
 * To add trades to the merchant, simply add new trades to the list. {@link BasicTrade} provides a default implementation.
*/
public class WandererTradesEvent extends Event
{

    protected List<ITrade> generic;
    protected List<ITrade> rare;

=====================================================
/**
 * Event that hooks into GameRenderer, allowing any feature to customize visual attributes
 *  the player sees.
 */
public abstract class EntityViewRenderEvent extends net.minecraftforge.eventbus.api.Event
{
    private final GameRenderer renderer;
    private final ActiveRenderInfo info;
    private final double renderPartialTicks;

===========================
 private static class FogEvent extends EntityViewRenderEvent
    {
        private final FogType type;
===========================
**
     * Event that allows any feature to customize the fog density the player sees.
     * NOTE: In order to make this event have an effect, you must cancel the event
     */
    @Cancelable
    public static class FogDensity extends FogEvent
    {
        private float density;

===========================
/**
     * Event that allows any feature to customize the rendering of fog.
     */
    @HasResult
    public static class RenderFogEvent extends FogEvent
    {
        private final float farPlaneDistance;
===========================
 /**
     * Event that allows any feature to customize the color of fog the player sees.
     * NOTE: Any change made to one of the color variables will affect the result seen in-game.
     */
    public static class FogColors extends EntityViewRenderEvent
    {
        private float red;
        private float green;
        private float blue;
===========================
/**
     * Event that allows mods to alter the angles of the player's camera. Mainly useful for applying roll.
     */
    public static class CameraSetup extends EntityViewRenderEvent
    {
        private float yaw;
        private float pitch;
        private float roll;
===========================
 /**
     * Event that allows mods to alter the raw FOV itself.
     * This directly affects to the FOV without being modified.
     * */
    public static class FOVModifier extends EntityViewRenderEvent
    {
        private double fov;
=====================================================
public class ServerLifecycleEvent extends Event
{

    protected final MinecraftServer server;
=====================================================
/**
 * RegistryEvent supertype.
 */
public class RegistryEvent<T extends IForgeRegistryEntry<T>> extends GenericEvent<T>
{

===========================
 /**
     * Register new registries when you receive this event, through the {@link RecipeBuilder}
     */
    public static class NewRegistry extends net.minecraftforge.eventbus.api.Event
    {
===========================
/**
     * Register your objects for the appropriate registry type when you receive this event.
     *
     * <code>event.getRegistry().register(...)</code>
     *
     * The registries will be visited in alphabetic order of their name, except blocks and items,
     * which will be visited FIRST and SECOND respectively.
     *
     * ObjectHolders will reload between Blocks and Items, and after all registries have been visited.
     * @param <T> The registry top level type
     */
    public static class Register<T extends IForgeRegistryEntry<T>> extends RegistryEvent<T>
    {
        private final IForgeRegistry<T> registry;
        private final ResourceLocation name;
===========================
public static class MissingMappings<T extends IForgeRegistryEntry<T>> extends RegistryEvent<T>
    {
        private final IForgeRegistry<T> registry;
        private final ResourceLocation name;
        private final ImmutableList<Mapping<T>> mappings;
        private ModContainer activeMod;

=====================================================
/**
 * Event class for handling GuiContainer specific events.
 */
public class GuiContainerEvent extends Event
{

    private final ContainerScreen guiContainer;

===========================
/**
     * This event is fired directly after the GuiContainer has draw any foreground elements,
     * But before the "dragged" stack, and before any tooltips.
     * This is useful for any slot / item specific overlays.
     * Things that need to be on top of All GUI elements but bellow tooltips and dragged stacks.
     */
    public static class DrawForeground extends GuiContainerEvent
    {
        private final int mouseX;
        private final int mouseY;
===========================
/**
     * This event is fired directly after the GuiContainer has draw any background elements,
     * This is useful for drawing new background elements.
     */
    public static class DrawBackground extends GuiContainerEvent
    {
        private final int mouseX;
        private final int mouseY;

=====================================================
/**
 * This event is called before any Gui will open.
 * If you don't want this to happen, cancel the event.
 * If you want to override this Gui, simply set the gui variable to your own Gui.
 * 
 * @author jk-5
 */
@net.minecraftforge.eventbus.api.Cancelable
public class GuiOpenEvent extends net.minecraftforge.eventbus.api.Event
{
    private Screen gui;
=====================================================
/**
 * Client side player connectivity events.
 */
public class ClientPlayerNetworkEvent extends Event {
    private final PlayerController controller;
    private final ClientPlayerEntity player;
    private final NetworkManager networkManager;

===========================
/**
     * Fired when the client player logs in to the server. The player should be initialized.
     */
    public static class LoggedInEvent extends ClientPlayerNetworkEvent {

===========================
 /**
     * Fired when the player logs out. Note this might also fire when a new integrated server is being created.
     */
    public static class LoggedOutEvent extends ClientPlayerNetworkEvent {

===========================

    /**
     * Fired when the player object respawns, such as dimension changes.
     */
    public static class RespawnEvent extends ClientPlayerNetworkEvent {
        private final ClientPlayerEntity oldPlayer;

=====================================================
/**
 * Fired when you should call {@link net.minecraft.client.particle.ParticleManager#registerFactory}.
 * Note that your {@code ParticleType}s should still be registered during the usual registry events, this
 * is only for the factories.
 */
public class ParticleFactoryRegisterEvent extends Event {}
=====================================================
/**
 * EntityEvent is fired when an event involving any Entity occurs.<br>
 * If a method utilizes this {@link net.minecraftforge.eventbus.api.Event} as its parameter, the method will
 * receive every child event of this class.<br>
 * <br>
 * {@link #entity} contains the entity that caused this event to occur.<br>
 * <br>
 * All children of this event are fired on the {@link MinecraftForge#EVENT_BUS}.<br>
 **/
public class EntityEvent extends Event
{
    private final Entity entity;


===========================
/**
     * EntityConstructing is fired when an Entity is being created. <br>
     * This event is fired within the constructor of the Entity.<br>
     * <br>
     * This event is not {@link net.minecraftforge.eventbus.api.Cancelable}.<br>
     * <br>
     * This event does not have a result. {@link HasResult}<br>
     * <br>
     * This event is fired on the {@link MinecraftForge#EVENT_BUS}.<br>
     **/
    public static class EntityConstructing extends EntityEvent
    {
===========================
/**
     * CanUpdate is fired when an Entity is being created. <br>
     * This event is fired whenever vanilla Minecraft determines that an entity<br>
     * cannot update in {@link World#updateEntityWithOptionalForce(net.minecraft.entity.Entity, boolean)} <br>
     * <br>
     * {@link CanUpdate#canUpdate} contains the boolean value of whether this entity can update.<br>
     * If the modder decides that this Entity can be updated, they may change canUpdate to true, <br>
     * and the entity with then be updated.<br>
     * <br>
     * This event is not {@link Cancelable}.<br>
     * <br>
     * This event is fired on the {@link MinecraftForge#EVENT_BUS}.<br>
     **/
    public static class CanUpdate extends EntityEvent
    {
        private boolean canUpdate = false;
===========================
 /**
     * EnteringChunk is fired when an Entity enters a chunk. <br>
     * This event is fired whenever vanilla Minecraft determines that an entity <br>
     * is entering a chunk in {@link Chunk#addEntity(net.minecraft.entity.Entity)} <br>
     * <br>
     * This event is not {@link Cancelable}.<br>
     * <br>
     * This event does not have a result. {@link HasResult}
     * <br>
     * This event is fired on the {@link MinecraftForge#EVENT_BUS}.<br>
     **/
    public static class EnteringChunk extends EntityEvent
    {
        private int newChunkX;
        private int newChunkZ;
        private int oldChunkX;
        private int oldChunkZ;

===========================
/**
     * EyeHeight is fired when an Entity's eye height changes. <br>
     * This event is fired whenever the {@link Pose} changes, and in a few other hardcoded scenarios.<br>
     * <br>
     * This event is not {@link Cancelable}.<br>
     * <br>
     * This event does not have a result. {@link HasResult}
     * <br>
     * This event is fired on the {@link MinecraftForge#EVENT_BUS}.<br>
     **/
    public static class EyeHeight extends EntityEvent
    {
        private final Pose pose;
        private final EntitySize size;
        private final float oldHeight;
        private float newHeight;
     
=====================================================
/**
 * ChunkWatchEvent is fired when an event involving a chunk being watched occurs.<br>
 * If a method utilizes this {@link Event} as its parameter, the method will
 * receive every child event of this class.<br>
 * <br>
 * {@link #pos} contains the ChunkPos of the Chunk this event is affecting.<br>
 * {@link #world} contains the World of the Chunk this event is affecting.<br>
 * {@link #player} contains the EntityPlayer that is involved with this chunk being watched. <br>
 * <br>
 * The {@link #player}'s world may not be the same as the world of the chunk
 * when the player is teleporting to another dimension.<br>
 * <br>
 * All children of this event are fired on the {@link MinecraftForge#EVENT_BUS}.<br>
 **/
public class ChunkWatchEvent extends Event
{
    private final ServerWorld world;
    private final ServerPlayerEntity player;
    private final ChunkPos pos;

===========================
/**
     * ChunkWatchEvent.Watch is fired when an EntityPlayer begins watching a chunk.<br>
     * This event is fired when a chunk is added to the watched chunks of an EntityPlayer in
     * {@link net.minecraft.world.server.ChunkManager#setChunkLoadedAtClient}. <br>
     * <br>
     * This event is not {@link net.minecraftforge.eventbus.api.Cancelable}.<br>
     * <br>
     * This event does not have a result. {@link HasResult} <br>
     * <br>
     * This event is fired on the {@link MinecraftForge#EVENT_BUS}.<br>
     **/
    public static class Watch extends ChunkWatchEvent
    {
===========================
 /**
     * ChunkWatchEvent.UnWatch is fired when an EntityPlayer stops watching a chunk.<br>
     * This event is fired when a chunk is removed from the watched chunks of an EntityPlayer in
     * {@link net.minecraft.world.server.ChunkManager#setChunkLoadedAtClient}. <br>
     * <br>
     * This event is not {@link net.minecraftforge.eventbus.api.Cancelable}.<br>
     * <br>
     * This event does not have a result. {@link HasResult} <br>
     * <br>
     * This event is fired on the {@link MinecraftForge#EVENT_BUS}.<br>
     **/
    public static class UnWatch extends ChunkWatchEvent
    {
=====================================================
```
# Registries
Registries are minecraft's way of keeping a list of all the tings that exists. So things like: Blocks, Items, Mobs, etc.. All registries are closed after the game loads so we have to add our new items in there while the game is loading up. For this forge provides functions that get called when a registry is being loaded up.
There is also a way for us to make new registry types (like spells) and other mods can also add items to these so people can make extension mods (if we allow it by making the classes available).